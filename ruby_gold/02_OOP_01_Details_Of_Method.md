### Fundamentals of Methods
#### Defining Methods
- Syntax and conventions:
```ruby
def method_name(parameters)
  # method body
end
```

- Single-line and multi-line methods
```ruby
# single-line method
def method_name(arguments); end

# multi-line method
def method_name(arguments)
  # method body
end
```

- Keyword arguments vs. positional arguments.
```ruby
def method_name(keyword_argument: default_value)
  # method body
end

def method_name(positional_argument, keyword_argument: default_value)
  # method body
end
```

#### Calling Methods
- Implicit and explicit receiver (self vs. direct call).
```ruby
# Implicit receiver
self.method_name(arguments)

# Explicit receiver
receiver.method_name(arguments)
```

### Method Arguments
#### Default Arguments
```ruby
def method_name(arg1 = default_value1, arg2 = default_value2)
  # method body
end
```

#### Keyword Arguments
- Required and optional keyword arguments.
- Double splat operator for dynamic keywords.
```ruby
def configure(**options)
  options[:theme] || "default"
end
```

```ruby
def foo(arg1:100, arg2:200)
  puts arg1
  puts arg2
end

option = {arg2: 900}

foo arg1: 200, **option # => 200, 900
```

#### Variable-length Arguments
```ruby
def sum(*numbers)
  numbers.reduce(0, :+)
end
```

### Method Return Values
#### Implicit Returns
Ruby methods return the last evaluated expression.
```ruby
def add(a, b)
  a + b
end

puts add(1, 2) # => 3
```

#### Explicit Returns
Using return for clarity or early exits.

#### Chained Method Calls
Ensuring methods return self for fluent interfaces.
```ruby
class MyClass
  def method_chain
    self
  end
end
```

### Method Visibility
#### Public, Private, Protected
- Differences and best practices.
- Using private and protected in context.
```ruby
private
def helper_method
  # only callable within the class
end
```

### Advanced Method Techniques
#### Method Overloading
- Simulating overloading using argument handling.
```ruby
def method_name(arg1, arg2 = nil)
  # method body
end
```

#### Method Overriding
- Overriding inherited methods in subclasses.
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

#### Method Aliasing
- Using alias and alias_method to create method shortcuts or extend functionality.
```ruby
alias_method :old_method, :new_method

alias_method old_method, new_method
```

### Metaprogramming with Methods
#### Reflection
- Listing and inspecting methods with methods, instance_methods, private_methods.
```ruby
class MyClass
  def public_method
    # method body
  end

  private
  def private_method
    # method body
  end
end
```

- Checking if a method is defined with respond_to?.
```ruby
respond_to?(:method_name)
```

#### Method Objects
- Using method to retrieve a Method object and call it.
```ruby
method(:method_name).call
```

### Performance Considerations
#### Avoiding Overhead
- Optimize frequently called methods.
- Use memoization to store expensive results.
```ruby
def expensive_method
  @result ||= heavy_computation
end
```

#### Benchmarking
- Profiling method performance using Benchmark module.
```ruby
require 'benchmark'

Benchmark.bm do |x|
  x.report { expensive_method }
end
```
