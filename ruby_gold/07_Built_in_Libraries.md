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
[Key Concepts](../ruby_silver/02_Built-in_Libraries_03_String.md#regexp)

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

- $1 is a global variable which can be used in later code:
```ruby
 if "foobar" =~ /foo(.*)/ then 
    puts "The matching word was #{$1}"
 end
```
- `$1`, `$2`, `$...` are the global-variables used by some of the ruby library functions specially concerning REGEX to let programmers use the findings in later codes.
```ruby
%r|(http://www(\.)(.*)/)| =~ "http://www.abc.com/"
# Return value: 0 because it matches the pattern

# $1 = "http://www.abc.com/" → Full match
# $2 = "." → Dot
# $3 = "abc.com" → After www.
```

#### Extracting Matches with scan
```ruby
"hello world".scan(/\w+/)  # => ["hello", "world"]
"abc 123 def 456".scan(/\d+/)  # => ["123", "456"]
```
**note**: scan does not perform overlapping matches
```ruby
p "x1x3x5x7".scan(/..[1-5]./) # => ["1x3x"]
p "x1x3x5x7".scan(/.[1-5]./) # => ["x1x", "x5x"]
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

### Enumerator
- A class that implements enumeration methods
- Can wrap any object to provide enumeration
- Doesn't require the object to include Enumerable

```ruby
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

- Cannot call using in a class because it will raise an error
```ruby
class C
  using M
  def m1
    'C.m1 in M'
  end
end
```

- Cannot call using in a method because it will raise an error
```ruby
class C
  using M
  def m1
    using M
  end
end

C.new.m1
```

- Using just apply last refine
```ruby
class C
  def m1(value)
    100 + value
  end
end

module R1
  refine C do
    def m1
      super 50
    end
  end
end

module R2
  refine C do
    def m1
      super 100
    end
  end
end

using R1
using R2

# => Just apply R2, R1 is not applied
C.new.m1 # => 200
```

## Other
### dup and clone
- Both Object#dup and Object#clone produce shallow copies. In the case of an Array, this means that the array itself is copied, but the objects within the array are not. So in this specific exaple original[0] and copy[0] both still reference the same object.

- One convention that can be used for creating deep copies in Ruby is to serialize and then deserialize an object, using the Marsal core class, e.g. copy = Marsal.load(Marhal.dump(original)).

- Object#dup does not copy an object's singleton methods. But clone does.
```ruby
obj = Object.new

def obj.hello
  puts "Hi!"
end

copy = obj.clone

copy.hello
# => Hi!

copy_dup = obj.dup

copy_dup.hello
# => NoMethodError (undefined method `hello' for #<Object:0x0000000100000000>)
```

- Marshal.dump is unable to serialize objects that have singleton methods defined on them.

- There is no Object#copy method.

### to_s
Many Ruby methods (including Kernel#puts) call to_s in order to convert objects into a string representation. The default implementation of Object#to_s produces simple, generic output which looks like this:

#<ShoppingList:0x007fb651918610>

When the to_s method is overidden in other objects, it can be used to provide a better string representation of the object, as shown in this question.

```ruby
class ShoppingList
  def initialize(items)
    @items = items
  end

  def to_s
    @items.join(", ")
  end
end

list = ShoppingList.new(["apple", "banana", "cherry"])
puts list
# => apple, banana, cherry
```

### inspect
The Kernel#p method calls inspect on its arguments in order to produce strings that can be used for debugging purposes. The default functionality of Object#inspect lists provides basic useful information, but the output can be customized by overriding the method in specific classes or objects.

```ruby
class ShoppingList
  def initialize(items)
    @items = items
  end

  def inspect
    @items.join(", ")
  end
end

list = ShoppingList.new(["apple", "banana", "cherry"])
p list
# => #<ShoppingList:0x007fb651918610 @items=["apple", "banana", "cherry"]>
```

### to_str
The to_str method is rarely implemented in practice, because it is only useful for situations in which you have an object that closely maps to Ruby's String concept, but is not a String itself. So while this method might be implemented by certain low-level data structures, it is not meant to be used for similar purposes to what to_s and inspect are used for.

