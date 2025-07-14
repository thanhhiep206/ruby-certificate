# Advanced Syntax Features

## Operators and Precedence

### Ruby Operator Precedence
List of Ruby operators in order of precedence (from highest to lowest):

1. **Parentheses**: `()` - Used to explicitly group expressions or change precedence
2. **Method calls**: Without parentheses (e.g., `obj.method arg`)
3. **Unary Operators**: `!`, `~`, `+`, `-` (logical NOT, bitwise NOT, unary plus, unary minus)
4. **Exponentiation**: `**`
5. **Multiplicative**: `*`, `/`, `%` (multiplication, division, modulo)
6. **Additive**: `+`, `-` (addition, subtraction)
7. **Bitwise Shifts**: `<<`, `>>` (left shift, right shift)
8. **Bitwise AND**: `&`
9. **Bitwise OR, XOR**: `|`, `^` (bitwise OR, bitwise XOR)
10. **Comparison Operators**: `<`, `<=`, `>`, `>=`, `<=>` (spaceship operator)
11. **Equality Operators**: `==`, `===`, `!=`, `=~`, `!~` (equality, pattern matching)
12. **Logical AND**: `&&`
13. **Logical OR**: `||`
14. **Range Operators**: `..`, `...` (inclusive and exclusive ranges)
15. **Ternary Conditional**: `? :` (ternary operator)
16. **Assignment**: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `**=`, `<<=`, `>>=`, `&=`, `|=`, `^=`
17. **Defined? Operator**: `defined?`
18. **Logical not**: `not` (lower precedence than `&&` and `||`)
19. **Logical or, and**: `or`, `and` (lower precedence than `||` and `&&`)
20. **Modifiers**: `if`, `unless`, `while`, `until`
21. **Statement Modifiers**: `begin...end` blocks with `rescue`, `ensure`

### Important Notes
- `and` and `or` have lower precedence than `&&` and `||`. Use `&&`/`||` for logical operations to avoid unexpected behavior:
```ruby
a = true and false  # Assigns `true` to `a`, then evaluates `and false`
a = true && false   # Assigns `false` to `a`
```

- Block precedence with method calls:
```ruby
def m1(*)
  str = yield if block_given?
  p "m1 #{str}"
end

def m2(*)
  str = yield if block_given?
  p "m2 #{str}"
end

m1 m2 do
  "hello"
end
# => "m2 "
# => "m1 hello"

m1 m2 {
  "hello"
}
# => "m2 hello"
# => "m1"
```

## Blocks, Procs, and Lambdas

### Blocks
Blocks are chunks of code that can be passed to methods and executed.

#### Syntax
```ruby
# Multi-line blocks use do...end
[1, 2, 3].each do |num|
  puts num
end

# Single-line blocks use curly braces
[1, 2, 3].each { |num| puts num }
```

#### Key Characteristics
- Blocks are not objects (unlike Procs and lambdas)
- No need to name the block explicitly
- Blocks are closures - they can access variables from their parent scope

#### Yield Keyword
```ruby
def test
  yield if block_given?
end

test { puts "Hello, World!" }
```

#### Block Arguments
```ruby
def test(&block)
  block.call
end

test { puts "Hello, World!" }

# Block arguments must come last
def test(n, &block)
  block.call
end

test(5) { puts "Hello, World!" }

# This causes a syntax error:
# def test(&block, n)  # SyntaxError
```

#### Block Scoping and Variable Shadowing
```ruby
x = 10
3.times do |x|
  x = 100 # x inside block shadows outer x
end
puts x # => 10 (outer x unchanged)
```

### Procs
```ruby
square = Proc.new {|x| x**2 }

square.call(3)  #=> 9
# shorthands:
square.(3)      #=> 9
square[3]       #=> 9
```

```ruby
def gen_times(factor)
  Proc.new {|n| n*factor } # remembers the value of factor at the moment of creation
end

times3 = gen_times(3)
times5 = gen_times(5)

times3.call(12)               #=> 36
times5.call(5)                #=> 25
times3.call(times5.call(4))   #=> 60
```

- to_proc will return a proc that will call the method on the object
```ruby
:to_s.to_proc.call(1)           #=> "1"
[1, 2].map(&:to_s)              #=> ["1", "2"]

method(:puts).to_proc.call(1)   # prints 1
[1, 2].each(&method(:puts))     # prints 1, 2

{test: 1}.to_proc.call(:test)       #=> 1
%i[test many keys].map(&{test: 1})  #=> [1, nil, nil]
```

### Lambdas
Lambdas are a special type of Proc with stricter argument handling and different return behavior.

#### Creating Lambdas
```ruby
# Using -> syntax (stabby lambda)
my_lambda = ->(x) { x * 2 }
puts my_lambda.call(10)  # => 20
puts my_lambda[10]       # => 20

# Using lambda keyword
my_lambda = lambda { |x| x * 2 }
```

#### Advanced Lambda Features
```ruby
# Currying
add = ->(x, y) { x + y }
add_10 = add.curry[10]
puts add_10.call(5)  # => 15

# Multi-parameter lambdas
multiply = ->(x, y) { x * y }
puts multiply.call(3, 4)  # => 12

# Lambda cannot call with missing arguments
my_lambda = ->(x, y) { x * y }
my_lambda.call(10, 20)  # => 200
my_lambda.call(10)  # ArgumentError: wrong number of arguments (given 1, expected 2)
```

## Non-local Exits

### return
Used to exit a method and return a value. Can cause surprising behavior when used in blocks or lambdas.
```ruby
def test
  [1, 2, 3].each do |i|
    return "Exited early!" if i == 2
  end
  "Completed iteration."
end

puts test # => "Exited early!"
```

### break
`break` exits a loop immediately.
```ruby
for i in 1..5
  break if i == 3
  puts i
end
```

### next
`next` skips the rest of the current iteration and continues with the next one.
```ruby
for i in 1..5
  next if i == 3
  puts i
end
```

### throw and catch
Provides a way to jump to a predefined point in the program, similar to exceptions but without raising an error.
```ruby
catch(:exit_point) do
  [1, 2, 3].each do |i|
    throw :exit_point, "Exited early!" if i == 2
  end
end
```

## Pattern Matching (Ruby 3.0+)

### Basic Syntax
```ruby
case value
in pattern
  # do something
else
  # fallback
end
```

### Supported Patterns

#### Array Matching
```ruby
case [1, 2, 3]
in [1, 2, 3]
  puts "Exact match!"
in [1, 2, *rest]
  puts "Matches with rest: #{rest}"
end
```

#### Hash Matching
```ruby
case { name: "Ruby", version: "3.2" }
in { name: "Ruby", version: version }
  puts "Version: #{version}"  # => "Version: 3.2"
end
```

#### Constant Matching
```ruby
case "example"
in String
  puts "It's a string!"
end
```

#### Variable Binding
```ruby
case { a: 1, b: 2 }
in { a: x, b: y }
  puts x + y # => 3
end
```

#### Wildcard Matching (_)
```ruby
case [1, 2, 3]
in [_, 2, _]
  puts "Matched middle 2!"
end
```

### Advanced Matching

#### Guards
```ruby
case [1, 2]
in [a, b] if a < b
  puts "a is less than b"
end
```

#### Nested Patterns
```ruby
case { data: { user: { id: 42, name: "Alice" } } }
in { data: { user: { id: 42, name: name } } }
  puts name # => "Alice"
end
```

#### As Patterns (=>)
```ruby
case { a: 1, b: 2 }
in { a: 1, b: => value }
  puts value # => 2
end
```

## Numbered Parameters

### Basics
- `_1`, `_2`, `_3`, etc., are implicit block parameters that correspond to the first, second, third arguments passed to a block.
```ruby
[1, 2, 3].map { _1 * 2 } # => [2, 4, 6]
```

### Use Cases

#### Short and Simple Blocks
```ruby
(1..5).map { _1**2 } # => [1, 4, 9, 16, 25]
```

#### Methods with Multiple Parameters
```ruby
[[1, 2], [3, 4]].map { _1 + _2 } # => [3, 7]
```

### Important Notes
- Mixing numbered parameters (`_1`) with explicitly named ones causes a SyntaxError:
```ruby
[1, 2, 3].each { |x| puts _1 } # SyntaxError
```

## Here Documents
- https://docs.ruby-lang.org/ja/latest/doc/spec=2fliteral.html#here

### Basic Here Documents
```ruby
str = <<TEXT
This is a heredoc.
It can span multiple lines.
TEXT
puts str
```

### Squiggly Here Documents (Remove Leading Whitespace)
```ruby
str = <<~TEXT
  This is a heredoc.
  Leading spaces are removed.
TEXT
puts str
```

### Concatenating Here Documents
```ruby
str = <<TEXT1 + <<TEXT2
Hello from the first heredoc.
TEXT1
Hello from the second heredoc.
TEXT2
puts str
```

### Here Documents in Data Structures
```ruby
data = {
  title: <<TITLE,
Welcome to Ruby!
TITLE
  body: <<BODY
This is a detailed description.
BODY
}
puts data[:title]
puts data[:body]
```

### Method Calls on Here Documents
To call a method on a heredoc, place it after the opening identifier:
```ruby
puts(<<-ONE)
content for heredoc one
ONE

puts(<<-ONE
content for heredoc one
ONE
)

puts(<<-ONE, <<-TWO)
content for heredoc one
ONE
content for heredoc two
TWO
``` 

### Here Document with call method
```ruby
def hello
  puts <<-EOS.gsub(/^\s+/, '') # Remove leading spaces
    Hello,
    World!
  EOS
end

hello
# => Hello,
# => World!
```

## raise
- The ancestry chain for each of the classes listed in this question is as follows:
```ruby
ArgumentError < StandardError < Exception

ScriptError < Exception
```

If rescue is called without a specific error class, it will catch StandardError and its descendents by default. Most exceptions in core Ruby are descendents of StandardError, but there are some that are not usually meant to be rescued which exist in other class hierarchies which descend directly from the Exception base clase.
```ruby
begin
  raise ArgumentError # or raise StandardError or raise RuntimeError
rescue => e
  puts e.class
  puts "OK"
end

# [Execution Result]
# OK
```

- StandardError: Parent of most exceptions
```ruby
begin
  result = 10 / 0
rescue => e
  puts "Caught a StandardError: #{e.class} - #{e.message}"
end

begin
  result = 10 / 0
rescue StandardError => e
  puts "Caught a StandardError: #{e.class} - #{e.message}"
end
```

- Exception: Parent of all exceptions
```ruby
begin
  raise Exception, "Something went really wrong"
rescue Exception => e
  puts "Caught an Exception: #{e.class} - #{e.message}"
end

begin
  raise "Something went really wrong"
rescue Exception => e
  puts "Caught an Exception: #{e.class} - #{e.message}"
end
```

- NameError: When a variable or constant is undefined

- NoMethodError: When call a method that is not defined in the object

- ArgumentError: When call a method with wrong number of arguments

- re-raise
```ruby
CustomError = Class.new(StandardError)
begin
  raise CustomError, "Something went really wrong"
rescue => e
  puts "Caught an Exception: #{e.class} - #{e.message}"
  raise # re-raise the exception with CustomError
end

# => CustomError: Something went really wrong
```

## ensure
- The ensure branch of a method (or begin/end block) is always run, whether an exception was raised or not.

- However, there is no implicit return value from an ensure branch, and so instead the implicit return value is set to the result of expression that ran just before the ensure branch was executed.
```ruby
def greeting
  "hello"
ensure
  puts "Ensured called!"

  "hi"
end

puts greeting
# => Ensured called!
# => hi
```

## filter_map
Returns a new array containing the truthy results (everything except false or nil) of running the block for every element in enum.
