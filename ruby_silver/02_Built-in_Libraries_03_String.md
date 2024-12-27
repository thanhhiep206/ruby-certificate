### Substitution Methods
#### sub
One substitution (or none); returns a new string.

#### !sub 
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
"edadsad".slice!(1,  3)
# => "dad"
```

#### squeeze!
Removes contiguous duplicate characters; returns self.

#### delete!
Removes characters as determined by the intersection of substring arguments.
```ruby
"hello".delete!("el") # => "ho"
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
```

#### chomp!
Removes trailing newline characters; returns self if any changes, nil otherwise.
```ruby
"hello\n".chomp! # => "hello"
"hello".chomp!   # => "hello"
```

#### chop!
Removes the last character; returns self if any changes, nil otherwise.
```ruby
"hello".chop! # => "hell"
"hello!".chop! # => "hello"
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
