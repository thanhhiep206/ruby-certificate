### Boolean and Nil Literals
- nil and false are both false values
- true is a true value. All objects except nil and false evaluate to a true value in conditional expressions

### Number Literals
#### Integer Literals
- Can write integers of any size as follows: 1234, 1_234 (You may place an underscore anywhere in the number)
- Can use a special prefix to write numbers in decimal, hexadecimal, octal or binary formats (The alphabetic component of the number is not case-sensitive)
  - For decimal numbers use a prefix of 0d
  - For hexadecimal numbers use a prefix of 0x
  - For octal numbers use a prefix of 0 or 0o
  - For binary numbers use a prefix of 0b
```ruby
0d170
0D170

0xaa
0xAa
0xAA
0Xaa
0XAa
0XaA

0252
0o252
0O252

0b10101010
0B10101010
```
All these numbers have the same decimal value, 170. Like integers and floats you may use an underscore for readability.
dme lai phai hoc toan

### Float Literals
Floating-point numbers may be written as follows
```ruby
12.34
1234e-2
1.234E1
```
These numbers have the same value, 12.34

### String Literals
The most common way of writing strings is using ":
```ruby
"This is a string."
```

Escape sequences list:
- \n             newline (line feed), ASCII 0Ah (LF)
- \r             carriage return, ASCII 0Dh (CR)


Adjacent string literals are automatically concatenated by the interpreter:
```ruby
"con" "cat" "en" "at" "ion" #=> "concatenation"
```

Any combination of adjacent single-quote, double-quote, percent strings will be concatenated as long as a percent-string is not last.
```ruby
%q{a} 'b' "c" #=> "abc"
"a" 'b' %q{c} #=> NameError: uninitialized constant q
"a" %q{c} 'b' #=> Error: syntax error, unexpected 'b'
```

### Symbol Literals
A Symbol represents a name inside the ruby interpreter
```ruby
:"my_symbol1"
:"my_symbol#{1 + 1}"
:'my_symbol#{1 + 1}'
```

### Array Literals
- %w and %W: String-Array Literals
- %i and %I: Symbol-Array Literals

```ruby
%w(foo bar baz) # => ["foo", "bar", "baz"]

# use backslash to embed spaces in the strings
%w(foo\ bar baz\ bat) # => ["foo bar", "baz bat"]
```

### Hash Literals
A hash is created using key-value pairs between { and }:
Both the key and value may be any object.

Hash values can be omitted, meaning that value will be fetched from the context by the name of the key:
```ruby
x = 100
y = 200
h = { x:, y: }
#=> {:x=>100, :y=>200}
```

### Range Literals
A range represents an interval of values. The range may include or exclude its ending value.
```ruby
(1..2)  # includes its ending value
(1...2) # excludes its ending value
(1..)   # endless range, representing infinite sequence from 1 to Infinity
(..1)   # beginless range, representing infinite sequence from -Infinity to 1
```

### Lambda Proc Literals
A lambda proc can be created with ->:
```ruby
-> { 1 + 1 }
# or
->(v) { 1 + v }
```

### Non-Interpolable String Literals
You can write a non-interpolable string with %q. The created string is the same as if you created it with single quotes:
```ruby
%[foo bar baz]        # => "foo bar baz" # Using [].
%(foo bar baz)        # => "foo bar baz" # Using ().
%{foo bar baz}        # => "foo bar baz" # Using {}.
%<foo bar baz>        # => "foo bar baz" # Using <>.
%|foo bar baz|        # => "foo bar baz" # Using two |.
%:foo bar baz:        # => "foo bar baz" # Using two :.
%q(1 + 1 is #{1 + 1}) # => "1 + 1 is \#{1 + 1}" # No interpolation.
```

### % and %Q: Interpolable String Literals
You can write an interpolable string with %Q or with its alias %
```ruby
%[foo bar baz]       # => "foo bar baz"
%(1 + 1 is #{1 + 1}) # => "1 + 1 is 2" # Interpolation.
```

### %w and %W: String-Array Literals
You can write an array of strings with %w (non-interpolable) or %W (interpolable)
```ruby
%w[foo bar baz]       # => ["foo", "bar", "baz"]
%w[1 % *]             # => ["1", "%", "*"]
# Use backslash to embed spaces in the strings.
%w[foo\ bar baz\ bat] # => ["foo bar", "baz bat"]
%w(#{1 + 1})          # => ["\#{1", "+", "1}"]
%W(#{1 + 1})          # => ["2"]
```

### %i and %I: Symbol-Array Literals
You can write an array of symbols with %i (non-interpolable) or %I (interpolable)
```ruby
%i[foo bar baz]       # => [:foo, :bar, :baz]
%i[1 % *]             # => [:"1", :%, :*]
# Use backslash to embed spaces in the symbols.
%i[foo\ bar baz\ bat] # => [:"foo bar", :"baz bat"]
%i(#{1 + 1})          # => [:"\#{1", :+, :"1}"]
%I(#{1 + 1})          # => [:"2"]
```

### %s: Symbol Literals
You can write a symbol with %s
```ruby
:foo     # => :foo
:foo bar # => :"foo bar"
```

### Here Document
```ruby
a = <<ENDD
Ruby
Php
END
ENDD
puts a #=> Ruby\nPhp\nEND\n
```

if a code in '' will be executed as a code
```ruby
s1 = 'Ruby'
s2 = 'Php'
a = <<'END'
#{s1}
#{s2}
END
puts a #=> "\#{s1}\n\#{s2}\n"
```

can use whitespace in here document
```ruby
a = <<-END
  Ruby
  Php
END
puts a #=> "  Ruby\n  Php\n"
```

| Method    | Description                                                                                                       |
|-----------|-------------------------------------------------------------------------------------------------------------------|
| <<END     | This creates a plain heredoc where string interpolation is allowed                                                |
| <<'END'   | This creates a plain heredoc where string interpolation is not allowed                                            |
| <<-END    | This creates a plain heredoc where string interpolation is allowed and the ending delimiter (END) can be indented |
| <<-'END'  | This is an indented heredoc, but string interpolation is not allowed                                              |
| <<"END"   | String interpolation is allowed, and there is no special handling for indentation                                 |
| <<-"END"  | This is the same as <<-END. String interpolation is allowed, and the ending delimiter can be indented             |
| <<~END    | Remove leading whitespace from the lines of the heredoc, can execute code in the heredoc                          |
