# Built-in Libraries

## Regular Expressions

### Regular Expression Basics
Regular expressions provide powerful pattern matching capabilities in Ruby:

#### Syntax
```ruby
/pattern/           # Basic regex literal
%r{pattern}         # Alternative syntax (useful when pattern contains /)
Regexp.new("pattern") # Dynamic regex creation
```

#### Key Concepts
- **Special characters**: `.` `\d` `\w` `\s` and their uppercase counterparts for negation (`\D`, `\W`, `\S`)
- **Quantifiers**: `*`, `+`, `?`, `{m,n}`
- **Anchors**: `^`, `$`, `\b` for word boundaries
- **Escaping**: Using `\` to escape special characters
- **Character classes**: `[abc]`, `[^abc]`

### Using Regex in Ruby

#### Pattern Matching
```ruby
# =~ returns the index of the first match or nil
"hello" =~ /l+/  # => 2
"hello" =~ /z/   # => nil

# match returns a MatchData object or nil
match_data = /(\w+) (\w+)/.match("John Doe")
match_data[0]  # => "John Doe" (entire match)
match_data[1]  # => "John" (first capture group)
match_data[2]  # => "Doe" (second capture group)
```

#### Extracting Matches with scan
```ruby
"hello world".scan(/\w+/)  # => ["hello", "world"]
"abc 123 def 456".scan(/\d+/)  # => ["123", "456"]
```

#### String Substitution
```ruby
# sub replaces first occurrence
"hello".sub(/l/, 'L')   # => "heLlo"

# gsub replaces all occurrences
"hello".gsub(/l/, 'L')  # => "heLLo"

# Using blocks for dynamic replacement
"hello world".gsub(/\w+/) { |word| word.capitalize }  # => "Hello World"
```

#### Splitting Strings
```ruby
"one,two,three".split(/,/)  # => ["one", "two", "three"]
"a1b2c3".split(/\d/)        # => ["a", "b", "c"]
```

### Capturing Groups

#### Positional Captures
```ruby
match = /(\d+)-(\w+)/.match("123-abc")
match[1]  # => "123"
match[2]  # => "abc"
```

#### Named Captures
```ruby
match = /(?<digits>\d+)-(?<letters>\w+)/.match("123-abc")
match[:digits]   # => "123"
match[:letters]  # => "abc"
match['digits']  # => "123" (string keys also work)
```

### MatchData Object
```ruby
match = /(\w+),(\w+)/.match("before hello,world after")

# Access parts of the match
match.pre_match   # => "before "
match.post_match  # => " after"
match.captures    # => ["hello", "world"]
match.names       # => [] (or named capture names)
```

### Regex Modifiers
```ruby
/pattern/i    # Case insensitive
/pattern/m    # Multiline mode (. matches newline)
/pattern/x    # Ignore whitespace in the pattern
/pattern/o    # Evaluate the regex only once

# Example usage
"Hello".match?(/hello/i)  # => true (case insensitive)
```

### Advanced Patterns

#### Lookaheads and Lookbehinds
```ruby
# Positive lookahead: (?=...)
"hello".scan(/h(?=e)/)      # => ["h"] (h followed by e)

# Negative lookahead: (?!...)
"hello".scan(/h(?!i)/)      # => ["h"] (h not followed by i)

# Positive lookbehind: (?<=...)
"hello".scan(/(?<=h)e/)     # => ["e"] (e preceded by h)

# Negative lookbehind: (?<!...)
"hello".scan(/(?<!h)e/)     # => [] (e not preceded by h)
```

#### Backreferences
```ruby
# Referencing captured groups with \1, \2, etc.
/(.)\1/.match("book")       # => Match for "oo"
"hello".gsub(/(.)(.)/,'\2\1')  # => "ehllo" (swap characters)
```

### Regex Performance
- Avoid catastrophic backtracking with complex patterns
- Use lazy quantifiers (`*?`, `+?`) when appropriate
- Compile frequently used patterns once

## Proc and Lambda Details

### Creating Procs and Lambdas

#### Proc Creation
```ruby
# Using Proc.new
my_proc = Proc.new { |x| x * 2 }
puts my_proc.call(5) # => 10

# Using Kernel's proc method
my_proc = proc { |x| x ** 2 }
puts my_proc.call(3) # => 9

# From a block
def method_that_takes_block(&block)
  block  # This is now a Proc object
end

my_proc = method_that_takes_block { |x| x + 1 }
```

#### Lambda Creation
```ruby
# Using -> syntax (stabby lambda)
my_lambda = ->(x) { x + 1 }
puts my_lambda.call(4) # => 5

# Using lambda keyword
my_lambda = lambda { |x| x * 2 }
puts my_lambda.call(3) # => 6
```

### Differences Between Proc and Lambda

| Feature           | Proc                           | Lambda                             |
|-------------------|--------------------------------|------------------------------------|
| Argument Handling | Allows missing or extra args   | Enforces exact number of args     |
| return Behavior   | Exits from enclosing method    | Exits only from the lambda itself |
| Definition Syntax | `Proc.new` or `proc`           | `->` or `lambda`                   |

#### Argument Handling Example
```ruby
my_proc = Proc.new { |x, y| puts "#{x}, #{y}" }
my_proc.call(1)        # => "1, " (missing args become nil)

my_lambda = ->(x, y) { puts "#{x}, #{y}" }
# my_lambda.call(1)    # => ArgumentError: wrong number of arguments
```

#### Return Behavior Example
```ruby
def test_proc
  my_proc = Proc.new { return "from proc" }
  my_proc.call
  "never reached"
end

def test_lambda
  my_lambda = -> { return "from lambda" }
  my_lambda.call
  "this is reached"
end

test_proc    # => "from proc"
test_lambda  # => "this is reached"
```

### Proc Methods

#### Basic Methods
```ruby
p = Proc.new { |x| x + 2 }

# Different ways to call
p.call(5)  # => 7
p[5]       # => 7 (alternative syntax)
p.(5)      # => 7 (another alternative)
```

#### Introspection Methods
```ruby
p = ->(x, y = 1, *args, z:, **kwargs) { x + y }

# Get arity (number of required arguments)
p.arity  # => 1

# Get source location
p.source_location  # => ["file_path", line_number]

# Get parameter information
p.parameters
# => [[:req, :x], [:opt, :y], [:rest, :args], [:keyreq, :z], [:keyrest, :kwargs]]
```

### Use Cases for Procs

#### Callbacks
```ruby
def execute_callback(callback)
  puts "Before callback"
  result = callback.call
  puts "After callback"
  result
end

callback = Proc.new { puts "Callback executed!"; 42 }
execute_callback(callback)
```

#### Custom Iterators
```ruby
def my_each(array, &block)
  array.each { |element| block.call(element) }
end

my_each([1, 2, 3]) { |x| puts x * 2 }
```

#### Variable Capture
```ruby
var = 1
my_proc = Proc.new { puts var }
var = 2
my_proc.call  # => 2 (captures current value when called)
```

#### Dynamic Method Definitions
```ruby
class Calculator
  operations = {
    add: ->(a, b) { a + b },
    subtract: ->(a, b) { a - b },
    multiply: ->(a, b) { a * b }
  }
  
  operations.each do |name, operation|
    define_method(name) do |a, b|
      operation.call(a, b)
    end
  end
end

calc = Calculator.new
calc.add(2, 3)  # => 5
```

## Enumerator and Lazy Evaluation

### Understanding Enumerator
Enumerator provides external iteration over collections, offering more control over traversal and manipulation:

#### Creating Enumerators
```ruby
# From existing collections
array = [1, 2, 3]
enum = array.each   # Returns an Enumerator

# Custom enumerator
enum = Enumerator.new do |yielder|
  3.times { |i| yielder << i }
end
enum.to_a  # => [0, 1, 2]
```

### Enumerator Methods

#### Basic Iteration
```ruby
enum = [10, 20, 30].each

# Manual iteration
enum.next   # => 10
enum.next   # => 20
enum.next   # => 30
# enum.next # => StopIteration error

# Reset enumerator
enum.rewind
enum.next   # => 10 (back to start)
```

#### Transformation and Filtering
```ruby
enum = (1..5).to_enum

# Chain operations
result = enum.map { |x| x * 2 }.select { |x| x > 5 }.to_a
# => [6, 8, 10]

# Without materializing intermediate results
enum.lazy.map { |x| x * 2 }.select { |x| x > 5 }.first(2)
# => [6, 8]
```

### Lazy Enumerators
Lazy evaluation delays computation until explicitly required:

#### Creating Lazy Enumerators
```ruby
# Convert to lazy
lazy_enum = (1..1000).lazy

# Infinite sequences
infinite_enum = (1..Float::INFINITY).lazy
```

#### Lazy vs Eager Operations
```ruby
# Eager: processes all elements immediately
(1..1000).map { |x| x * 2 }.select { |x| x > 500 }.first(5)

# Lazy: processes elements as needed
(1..1000).lazy.map { |x| x * 2 }.select { |x| x > 500 }.first(5)
```

| Method   | Execution Type | Returns Immediately?              | Requires .to_a / .force for Output? |
|----------|----------------|-----------------------------------|-------------------------------------|
| first(n) | Eager          | Yes, returns an array             | No                                  |
| take(n)  | Lazy           | No, returns another lazy enum     | Yes                                 |

#### Get value from lazy enumerator
```ruby
(1..100).each.lazy.chunk(&:even?).first(5)
# => [[false, [1]], [true, [2]], [false, [3]], [true, [4]], [false, [5]]]

(1..100).each.lazy.chunk(&:even?).take(5).force
# => [[false, [1]], [true, [2]], [false, [3]], [true, [4]], [false, [5]]]
```

#### Infinite Sequences
```ruby
# Fibonacci sequence
fibonacci = Enumerator.new do |yielder|
  a, b = 0, 1
  loop do
    yielder << a
    a, b = b, a + b
  end
end.lazy

fibonacci.first(10)  # => [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]

# Prime numbers
require 'prime'
Prime.lazy.first(10)  # => [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
```

### Custom Enumerators

#### Using Enumerator::Generator
```ruby
# Custom generator
even_numbers = Enumerator.new do |yielder|
  num = 0
  loop do
    yielder.yield(num) if num.even?
    num += 1
  end
end

even_numbers.lazy.first(5)  # => [0, 2, 4, 6, 8]
```

#### File Processing Example
```ruby
def lines_from_file(filename)
  Enumerator.new do |yielder|
    File.foreach(filename) do |line|
      yielder << line.chomp
    end
  end
end

# Process large files lazily
lines_from_file("large_file.txt").lazy
  .select { |line| line.include?("ERROR") }
  .first(10)
```

#### Yielding
Yielding is a way to pass control back to the block.
```ruby
def bar(&block)
  block.yield
end

bar do
  puts "hello, world"
end
# => hello, world
```

### Enumerator vs Enumerable

#### Enumerable
- A mixin module providing traversal capabilities
- Included in Array, Hash, Range, etc.
- Requires `each` method to be defined

#### Enumerator
- A class that implements enumeration methods
- Can wrap any object to provide enumeration
- Doesn't require the object to include Enumerable

```ruby
# Custom class with Enumerable
class TodoList
  include Enumerable
  
  def initialize
    @items = []
  end
  
  def each
    @items.each { |item| yield(item) }
  end
  
  def <<(item)
    @items << item
  end
end

# Using Enumerator for external iteration
class SimpleCollection
  def initialize(*items)
    @items = items
  end
  
  def each
    return enum_for(:each) unless block_given?
    @items.each { |item| yield(item) }
  end
end

collection = SimpleCollection.new(1, 2, 3)
enum = collection.each  # Returns Enumerator
enum.map(&:to_s)       # => ["1", "2", "3"]
```

### Performance Considerations
- **Lazy evaluation** reduces memory usage for large datasets
- **Enumerators** provide memory-efficient iteration
- **Chain operations** without creating intermediate arrays
- **Use `first(n)`** instead of `take(n).to_a` for better performance 

### Module#refine
- Module#refine will create a self anonymous module
```ruby
class C
end

module M
  refine C do
    self # anonymous module
  end
end
```

- If want to define self, should use singleton class
```ruby
class C
  def self.m1
    'C.m1'
  end
end

module M
  refine C.singleton_class do
    def m1
      'C.m1 in M'
    end
  end
end

using M

puts C.m1 # C.m1 in M
```
