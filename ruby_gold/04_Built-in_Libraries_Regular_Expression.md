### Regular Expressions Basics in Ruby
#### Key Concepts
- Syntax:
```ruby
/pattern/ 
```
- Special characters: . \d \w \s and their uppercase counterparts for negation (\D, \W, \S).
- Quantifiers: *, +, ?, {m,n}.
- Anchors: ^, $, \b for word boundaries.
- Escaping: Using \ to escape special characters.
- Character classes: [abc], [^abc].

### Using Regex in Ruby
#### Pattern Matching: =~ and match
- =~ returns the index of the first match or nil.
```ruby
"hello" =~ /l+/  # => 2
```

- match returns a MatchData object or nil.
```ruby
/(\w+) (\w+)/.match("John Doe") # => MatchData object
```

#### Scan: Extracting Matches
```ruby
"hello world".scan(/\w+/)  # => ["hello", "world"]
```

#### Substitution: sub and gsub
```ruby
"hello".sub(/l/, 'L')   # => "heLlo"
"hello".gsub(/l/, 'L')  # => "heLLo"
```

#### Splitting Strings
```ruby
"one,two,three".split(/,/)  # => ["one", "two", "three"]
```

#### Captures: Named and Positional
- Positional:
```ruby
/(\d+)-(\w+)/.match("123-abc")[1]  # => "123"
```

- Named:
```ruby
/(?<digits>\d+)-(?<letters>\w+)/.match("123-abc")[:digits]  # => "123"
```

### MatchData Object
- MatchData#captures: Array of captured groups.
- MatchData#[]: Access specific groups.
- MatchData#pre_match and MatchData#post_match: Parts of the string before and after the match.
```ruby
m = /(\w+),(\w+)/.match("hello,world")
m.pre_match  # => ""
m.post_match # => ""
```

### Regex Modifiers
#### Common Modifiers:
- i: Case-insensitive matching.
- m: Multiline mode (. matches newline
- x: Ignore whitespace in the pattern.
- o: Evaluate the regex only once.

```ruby
/pattern/i    # Case insensitive
```

### Advanced Patterns
#### Lookaheads and Lookbehinds:
- Positive lookahead: (?=...).
- Negative lookahead: (?!...).
- Positive lookbehind: (?<=...).
- Negative lookbehind: (?<!...).

```ruby
"hello".scan(/(?<=h)e/)  # => ["e"]
```

#### Backreferences:
- Referencing captured groups with \1, \2, etc.
```ruby
/(.)\1/.match("book")  # => Match for "oo"
```

### Regex in Common Libraries
#### ActiveSupport (Rails):
- ActiveSupport::Inflector: Inflections and regex usage
```ruby
ActiveSupport::Inflector.inflections do |inflect|
  inflect.acronym 'API'
end
```

#### File Operations:
- Dir.glob supports regex for file matching
```ruby
Dir.glob("*.{rb,erb}") # Matches Ruby and ERB files
```

### Regex Performance Optimization
- Avoid catastrophic backtracking with complex patterns.
- Use lazy quantifiers (*?, +?) when appropriate.


