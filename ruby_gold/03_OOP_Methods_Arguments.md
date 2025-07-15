# Object-Oriented Programming: Methods and Arguments

## Method Arguments

### Default Arguments
```ruby
def greet(name, greeting = "Hello")
  "#{greeting}, #{name}!"
end

greet("Alice")           # => "Hello, Alice!"
greet("Bob", "Hi")       # => "Hi, Bob!"
```

### Keyword Arguments
```ruby
# Required and optional keyword arguments
def configure(host:, port: 80, ssl: false)
  { host: host, port: port, ssl: ssl }
end

configure(host: "localhost")
configure(host: "example.com", port: 443, ssl: true)

# Double splat for dynamic keywords
def handle_options(**options)
  options.each { |key, value| puts "#{key}: #{value}" }
end

# Mixing arguments
def complex_method(required, optional = "default", *rest, keyword:, **options)
  # method body
end
```

### Variable-length Arguments (Splat)
```ruby
def sum(*numbers)
  numbers.reduce(0, :+)
end

sum(1, 2, 3, 4)  # => 10

# Keyword splat example
def foo(arg1: 100, arg2: 200)
  puts arg1
  puts arg2
end

option = { arg2: 900 }
foo(arg1: 200, **option)  # => 200, 900
```

## Method Return Values

### Implicit Returns
Ruby methods return the last evaluated expression.
```ruby
def add(a, b)
  a + b  # Last expression is returned
end

puts add(1, 2) # => 3
```

### Explicit Returns
Using return for clarity or early exits.
```ruby
def divide(a, b)
  return "Cannot divide by zero" if b == 0
  a / b
end
```

### Method Chaining
Ensuring methods return self for fluent interfaces.
```ruby
class FluentInterface
  def step_one
    puts "Step 1"
    self  # Return self for chaining
  end
  
  def step_two
    puts "Step 2"
    self
  end
end

FluentInterface.new.step_one.step_two
```

## Method Overloading and Overriding

### Method Overloading
Simulating overloading using argument handling.
```ruby
def method_name(arg1, arg2 = nil)
  if arg2.nil?
    # Handle single argument case
  else
    # Handle two argument case
  end
end
```

### Method Overriding
Overriding inherited methods in subclasses.
```ruby
class Parent
  def greet
    "Hello"
  end
end

class Child < Parent
  def greet
    "#{super}, I'm the child."
  end
end
```

- Can use super with parent private method
```ruby
class BaseClass
  private

  def greet
    puts "Hello World!"
  end
end

class ChildClass < BaseClass
  def greet
    super
  end
end

ChildClass.new.greet
# => Hello World!
```

## Method Aliasing
Using alias and alias_method to create method shortcuts or extend functionality.
Can alias a method (public, private, protected) or global variable.
```ruby
class User
  $a = 1
  def old_method
    @name
  end
  
  # Just use alias_method in class or module
  alias_method "new_method", "old_method"
  alias_method :new_method, :old_method
  alias :new_method :old_method
end

# Alternative syntax
# alias can use both inside and outside of class or module
alias new_method old_method
alias :new_method :old_method
alias $new_global_val $old_global_val
```

## Method Introspection

### Checking Method Existence
```ruby
class MyClass
  def public_method; end
  protected
  def protected_method; end
  private
  def private_method; end
end

obj = MyClass.new

# Check method existence
obj.respond_to?(:public_method)   # => true
obj.respond_to?(:private_method)  # => false
obj.respond_to?(:private_method, true)  # => true (include private)

# List methods
MyClass.instance_methods(false) # => [:public_method]
MyClass.private_instance_methods(false) # => [:private_method]
MyClass.protected_instance_methods(false) # => [:protected_method]
MyClass.methods.sort  # List all class methods
```

### Method Objects
Using method to retrieve a Method object and call it.
```ruby
method_obj = obj.method(:public_method)
method_obj.call

# Get method information
method_obj.arity          # Returns number of arguments
method_obj.source_location # Returns [file, line] where defined
method_obj.parameters     # Returns parameter information
```

### methods.include?
- methods is list public methods
```ruby
class MyClass
  def self.method_one
  end

  def method_two
  end
end

# singleton class methods
p MyClass.methods.include?(:method_one) # => true
p MyClass.methods.include?(:method_two) # => false

# instance methods
p MyClass.new.methods.include?(:method_one) # => false
p MyClass.new.methods.include?(:method_two) # => true
```

## Advanced Method Techniques

### Dynamic Method Definition
```ruby
class Calculator
  [:add, :subtract].each do |method|
    define_method(method) do |a, b|
      method == :add ? a + b : a - b
    end
  end
end

calc = Calculator.new
calc.add(2, 3) # => 5
calc.subtract(5, 3) # => 2
```

### Method Missing
```ruby
class DynamicGreeter
  def method_missing(name, *args, &block)
    if name.to_s.start_with?("greet_")
      "Hello, #{name.to_s.split('_').last.capitalize}!"
    else
      super
    end
  end
  
  def respond_to_missing?(method_name, include_private = false)
    method_name.to_s.start_with?('greet_') || super
  end
end

greeter = DynamicGreeter.new
greeter.greet_john # => "Hello, John!"
``` 

### Const Missing
```ruby
class Foo
  def self.const_missing(a)
    p "#{a}"
  end
end
Foo::B # => "B"
```
