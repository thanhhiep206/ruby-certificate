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
#### class_eval and module_eval
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

#### singleton_class
- Access an object’s singleton class to define methods specific to that object.
```ruby
obj = Object.new
obj.singleton_class.define_method(:unique_method) { "I'm unique!" }
obj.unique_method # => "I'm unique!"
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


