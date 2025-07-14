### Substitution Methods
#### sub
One substitution (or none); returns a new string.

#### sub! 
One substitution (or none); returns self.

#### gsub
Zero or more substitutions; returns a new string.

#### gsub!
Zero or more substitutions; returns self.

```ruby
s = 'hello'
s.sub(/[aeiou]/, '*')  # => "h*llo"
s.gsub(/[aeiou]/, '*') # => "h*ll*"
s.gsub(/[aeiou]/, '')  # => "hll"
s.sub(/ell/, 'al')     # => "halo"
s.gsub(/xyzzy/, '*')   # => "hello"
'THX1138'.gsub(/\d+/, '00') # => "THX00"
```

#### Regexp
- \& and \0 correspond to $&, which contains the complete matched text.
- \' corresponds to $', which contains string after match.
- \` corresponds to $`, which contains string before match.
- + corresponds to $+, which contains last capture group.

- . compare with single character
- .. compare with two characters
- [] [] any character in the range
- [^..] not any character in the range
- a-z from a to z
- \w match with any word character
- \W match with any non-word character
- \s match with any whitespace character
- \S match with any non-whitespace character
- \d match with any digit character
- \D match with any non-digit character
- \A match with the beginning of the string
- \$ match with the end of the string
- \Z match with the end of the string
- \z match with the end of the string
- \b match with a word boundary
- .* match with any character zero or more times
- ^ means matches the beginning of the string
- [x-y]+ means match with any character in the range x to y zero or more times


#### Hash replacement
If argument replacement is a hash, and pattern matches one of its keys, the replacing string is the value for that key:
```ruby
h = {'foo' => 'bar', 'baz' => 'bat'}
'food'.sub('foo', h) # => "bard"

h = {foo: 'bar', baz: 'bat'}
'food'.sub('foo', h) # => "d"
```

#### Block
In the block form, the current match string is passed to the block; the block's return value becomes the replacing string:
```ruby
s = '@'
'1234'.gsub(/\d/) {|match| s.succ! } # => "ABCD"
```

### Methods for Creating a String
#### ::new
Returns a new string.

#### ::try_convert
Returns a new string created from a given object.
```ruby
String.try_convert('hello') # => "hello"
String.try_convert(123)     # => nil
```

### Methods for a Frozen/Unfrozen String


#### string << object → string
Concatenates object to self and returns self:
```ruby
s = 'foo'
s << 'bar' # => "foobar"
s          # => "foobar"
```

#### string =~ regexp
Returns the Integer index of the first substring that matches the given regexp, or nil if no match found:
```ruby
'foo' =~ /f/ # => 0
'foo' =~ /o/ # => 1
'foo' =~ /x/ # => nil
```

### Methods for Querying

### Methods for Modifying a String
#### clear
Removes all content, so that self is empty; returns self.

#### slice!, []=
Removes a substring determined by a given index, start/length, range, regexp, or substring.
```ruby
s = "edadsad"
s.slice!(1,  3)
puts s
# => "esad"
```

#### squeeze!
Removes contiguous duplicate characters; returns self.

#### delete!
Removes characters as determined by the intersection of substring arguments.
```ruby
"hello".delete!("el") # => "ho"

"123456789".delete!("1-46-") # => "5789"

# Return a copy of the string with all characters in the intersection of its arguments deleted.
"01234".delete!("0-2", "1-3", "2-4") # => "0134"
```

#### lstrip!
Removes leading whitespace; returns self if any changes, nil otherwise.
```ruby
"  hello  ".lstrip! # => "hello  "
```

#### rstrip!
Removes trailing whitespace; returns self if any changes, nil otherwise.
```ruby
"  hello  ".rstrip! # => "  hello"
```

#### strip!
Removes leading and trailing whitespace; returns self if any changes, nil otherwise.
```ruby
"  hello  ".strip! # => "hello"
"Liberty Fish   \r\n".strip! # => "Liberty Fish"
```

#### chomp!
Removes trailing newline characters; returns self if any changes, nil otherwise.
```ruby
"hello\n".chomp! # => "hello"
"hello".chomp!   # => "hello"

# remove all newline characters
"12\r\n\r\n".chomp!("") # => "12"
```

#### chop!
Removes the last character; returns self if any changes, nil otherwise.
```ruby
"hello".chop! # => "hell"
"hello!".chop! # => "hello"
```

#### split
Splits self into substrings based on a delimiter, returning an array of these substrings.
```ruby
"hello".split # => ["hello"]
"hello world".split # => ["hello", "world"]
"hello world".split(' ') # => ["hello", "world"]
"A.S.A.P".split('.', 3) # => ["A", "S", "A.P"]
```

#### just
```ruby
"ab".rjust(6, "*") # => "****ab"
"ab".ljust(6, "*") # => "ab****"
"ab".center(6, "*") # => "**ab**"
```

### Methods for Converting to New String
#### string % object
Returns the result of formatting object into the format specification self
```ruby
"%05d" % 123 # => "00123"

# If self contains multiple substitutions, object must be an Array or Hash containing the values to be substituted:
"%-5s: %016x" % [ "ID", self.object_id ] # => "ID   : 00002b054ec93168"
"foo = %{foo}" % {foo: 'bar'} # => "foo = bar"
"foo = %{foo}, baz = %{baz}" % {foo: 'bar', baz: 'bat'} # => "foo = bar, baz = bat"
```

#### string * object
Returns a new string containing object repeated object.
```ruby
"Ho! " * 3 # => "Ho! Ho! Ho! "
"Ho! " * 0 # => ""
``` 

#### string + other_string
Returns a new String containing other_string concatenated to self:
```ruby
"Hello from " + self.to_s # => "Hello from main"
```

### Methods for Querying
#### index
Returns the index of the first occurrence of the given substring or pattern (regexp) in str. Returns nil if not found. If the second parameter is present, it specifies the position in the string to begin the search.
```ruby
"hello".index('e') # => 1
"hello".index('e', 2) # => nil
```

#### count
Returns the number of characters in self.
```ruby
"hello".count # => 5

# count with substring
"hello".count('e') # => 1

# This specifies a range of characters from 'a' to 'c' (inclusive), and count computes how many characters in the string fall within this range.
"abbxxcc".count('a-c') # => 5
```

### Convert to Integer

#### to_i
Returns the integer value of self.
```ruby
"123".to_i # => 123
```

```ruby
"123".to_i(2) # => 5
"123".to_i(8) # => 83
"123".to_i(16) # => 291
"123".to_i(36) # => 4359

"+5-3".to_i # => 5

"ABC".to_i # => 0
```

#### ord
Returns the integer ordinal of a one-character string.
```ruby
"A".ord # => 65
"a".ord # => 97
```

