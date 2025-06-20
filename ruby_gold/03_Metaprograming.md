### Dynamic Method Invocation
#### send and public_send
- Call methods dynamically by name.
```ruby
class User
  def greet
    "Hello!"
  end
end

user = User.new
user.send(:greet) # => "Hello!"
user.public_send(:greet) # => "Hello!"
```
Use public_send for public methods only, while send can call private methods.

#### method_missing
- Catch undefined method calls and handle them dynamically.
```ruby
class DynamicGreeter
  def method_missing(name, *args, &block)
    if name.to_s.start_with?("greet_")
      "Hello, #{name.to_s.split('_').last.capitalize}!"
    else
      super
    end
  end
end

greeter = DynamicGreeter.new
greeter.greet_john # => "Hello, John!"
greeter.unknown_method # Raises NoMethodError
```
Always call super for methods you don’t intend to handle.

- Class level method_missing
```ruby
class MyClass
  def self.method_missing(method_name, *arguments, &block)
    puts "method_missing: #{method_name}"
  end
end

MyClass.hoge # => "method_missing: hoge"
```

- The Class class is the superclass of all class objects in Ruby. If you call an undefined method on a class itself, method_missing in Class is invoked unless overridden.
```ruby
class Class
  def method_missing(id, *args)
    puts "Class#method_missing"
  end
end
class A
  def method_missing(id, *args)
    puts "A#method_missing"
  end
end
class B < A
  def method_missing(id, *args)
    puts "B#method_missing"
  end
end

B.dummy_method # => "Class#method_missing"
```

- respond_to_missing? for compatibility.
```ruby
require 'ostruct'

class Order
  def user
    @_user ||= OpenStruct.new(name: 'Mike', age: 28, occupation: 'slacker')
  end

  def method_missing(method_name, *arguments, &block)
    if method_name.to_s =~ /user_(.*)/
      user.send($1, *arguments, &block)
    else
      super
    end
  end

  def respond_to_missing?(method_name, include_private = false)
    method_name.to_s.start_with?('user_') || super
  end
end

order = Order.new
order.user_name => "Mike"
order.respond_to?(:user_name) => true
order.method(:user_name) => #<Method: Order#user_name>
```

### Defining Methods Dynamically
#### define_method
- Define methods dynamically at runtime
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
- Use this when you need to define many similar methods programmatically.

### Evaluating Code Dynamically
#### eval
- Execute a string as Ruby code.
```ruby
code = "3 + 2"
eval(code) # => 5
```
Avoid using eval unless absolutely necessary. It can lead to security vulnerabilities and hard-to-debug code.

- Safer alternatives include instance_exec or class_eval for more controlled code execution.
```ruby
instance_exec(1, 2) { |a, b| a + b } # => 3
class_eval(code) # => 5
```

#### class_eval
Evaluate code in the context of a class or module.

- Use class_eval with a block to define methods dynamically.
```ruby
class User
  class_eval do
    def greet
      "Hello!"
    end
  end
end
```

- Use class_eval with a string to evaluate code dynamically.
```ruby
class User
  class_eval("def greet; 'Hello!'; end")
end

User.new.greet # => "Hello!"
```

Difference between use a block and a string:
- Use a block when you need to define multiple methods or when the code is complex.
- Use a string when you need to evaluate simple code or when the code is short and simple.
```ruby
# Use a string will not be able to access the class or module context.
class C
  CONST = "Hello, world"
end

module M
  C.class_eval(<<-CODE)
    def awesome_method
      CONST
    end
  CODE
end

p C.new.awesome_method # => "Hello, world"
```

```ruby
# Use a block will be not able to access the class or module context.
class C
  CONST = "Hello, world"
end

module M
  C.class_eval do
    def awesome_method
      CONST
    end
  end
end

p C.new.awesome_method # => uninitialized constant M::CONST

# Use a string will be able to access the class or module context.
class C
  CONST = "Hello, world"
end

module M
  C.class_eval(<<-EOS)
    def awesome_method
      CONST
    end
  EOS
end

p C.new.awesome_method # => "Hello, world"
```


### Introspection and Reflection
#### instance_variable_get and instance_variable_set
- Access or set instance variables dynamically.
```ruby
class User
  def initialize(name)
    @name = name
  end
end

user = User.new("Alice")
user.instance_variable_get(:@name) # => "Alice"
user.instance_variable_set(:@name, "Bob")
user.instance_variable_get(:@name) # => "Bob"
```

#### methods, instance_methods, respond_to?
- Explore an object’s methods dynamically.
```ruby
user.methods.sort
user.respond_to?(:greet) # => true or false
```

#### const_get and const_set
- Dynamically retrieve or define constants.
```ruby
Object.const_set("MY_CONSTANT", 42)
Object.const_get("MY_CONSTANT") # => 42
```

### Class and Module Manipulation
#### class_eval
- Add methods or evaluate code in the context of a class or module.
```ruby
class User; end
User.class_eval do
  def greet
    "Hello!"
  end
end

User.new.greet # => "Hello!"
```

#### module_eval
- module_function
```ruby
# module_function will be able to define a method as a module function.
m.module_eval(<<-EOS)
  CONST = "Constant in Module instance"

  def const
    CONST
  end

  module_function :const # module_function にシンボルでメソッドを指定する
EOS
```

- instance_eval
```ruby
# instance_eval will be able to define a method as an instance method.
m.instance_eval(<<-EOS)
  CONST = "Constant in Module instance"

  def const
    CONST
  end
EOS
```

#### singleton_class
- Access an object’s singleton class to define methods specific to that object.
```ruby
obj = Object.new
obj.singleton_class.define_method(:unique_method) { "I'm unique!" }
obj.unique_method # => "I'm unique!"
```

```ruby
dog = "dog"
class << dog
  def bark
    "Woof!"
  end
end

puts dog.bark   # => "Woof!"
```

#### prepend and include
- Inject modules dynamically.
```ruby
module Greeter
  def greet
    "Hello from Greeter!"
  end
end

class User; end
User.include(Greeter)
User.new.greet # => "Hello from Greeter!"
```

### Custom DSLs (Domain-Specific Languages)
- Metaprogramming is often used to build DSLs. Rails uses metaprogramming extensively for things like has_many, validates, and scope.
```ruby
class Post
  attr_accessor :title

  def self.titleize
    define_method(:titleize) do
      @title.split.map(&:capitalize).join(' ')
    end
  end
end

Post.titleize
post = Post.new
post.title = "hello world"
post.titleize # => "Hello World"
```


