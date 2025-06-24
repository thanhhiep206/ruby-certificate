# Metaprogramming

## Dynamic Method Invocation

### send and public_send
Call methods dynamically by name:
```ruby
class User
  def greet
    "Hello!"
  end
  
  private
  
  def secret
    "Secret method"
  end
end

user = User.new
user.send(:greet) # => "Hello!"
user.public_send(:greet) # => "Hello!"

user.send(:secret) # => "Secret method" (can call private)
# user.public_send(:secret) # => NoMethodError (cannot call private)
```

**Use `public_send` for public methods only, while `send` can call private methods.**

### method_missing
Catch undefined method calls and handle them dynamically:
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
greeter.unknown_method # Raises NoMethodError
```

**Always call `super` for methods you don't intend to handle and implement `respond_to_missing?` for compatibility.**

### Class-level method_missing
```ruby
class MyClass
  def self.method_missing(method_name, *arguments, &block)
    puts "Class method_missing: #{method_name}"
  end
end

MyClass.hoge # => "Class method_missing: hoge"
```

### Method Missing with Class Hierarchy
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
B.new.dummy_method # => "B#method_missing"
```

**Note**: The Class class is the superclass of all class objects in Ruby.

## Defining Methods Dynamically

### define_method
Define methods dynamically at runtime:
```ruby
class Calculator
  [:add, :subtract, :multiply, :divide].each do |operation|
    define_method(operation) do |a, b|
      case operation
      when :add then a + b
      when :subtract then a - b
      when :multiply then a * b
      when :divide then b != 0 ? a / b : "Cannot divide by zero"
      end
    end
  end
end

calc = Calculator.new
calc.add(2, 3) # => 5
calc.multiply(4, 5) # => 20
```

**Use this when you need to define many similar methods programmatically.**

### Dynamic Method Definition with Metaprogramming
```ruby
class User
  ATTRIBUTES = [:name, :email, :age].freeze
  
  ATTRIBUTES.each do |attr|
    define_method("#{attr}_present?") do
      !instance_variable_get("@#{attr}").nil?
    end
    
    define_method("reset_#{attr}") do
      instance_variable_set("@#{attr}", nil)
    end
  end
end

user = User.new
user.instance_variable_set(:@name, "Alice")
user.name_present? # => true
user.reset_name
user.name_present? # => false
```

## Evaluating Code Dynamically

### eval
Execute a string as Ruby code:
```ruby
code = "3 + 2"
eval(code) # => 5

# Dynamic variable access
x = 10
eval("x * 2") # => 20
```

**Avoid using `eval` unless absolutely necessary. It can lead to security vulnerabilities and hard-to-debug code.**

### class_eval (module_eval)
Evaluate code in the context of a class or module:

#### Using with a Block
```ruby
class User
  class_eval do
    def greet
      "Hello from class_eval block!"
    end
  end
end

User.new.greet # => "Hello from class_eval block!"
```

#### Using with a String
```ruby
class User
  class_eval("def greet; 'Hello from class_eval string!'; end")
end

User.new.greet # => "Hello from class_eval string!"
```

### Difference between Block and String in class_eval

#### String Context
```ruby
class C
  CONST = "Hello, world"
end

module M
  C.class_eval(<<-CODE)
    CONST_IN_HERE_DOC = 100

    def awesome_method
      CONST  # Can access C's constants
    end
  CODE
end

C.new.awesome_method # => "Hello, world"

puts "HERE_DOC: CONST is defined? #{C.const_defined?(:CONST_IN_HERE_DOC, false)}" # true
puts "HERE_DOC: CONST is defined? #{Object.const_defined?(:CONST_IN_HERE_DOC, false)}" # false
```

#### Block Context
- if you pass a block to class_eval (or module_eval), the nesting is as follows:
```ruby
A.module_eval do
  p Module.nesting # [] # You are not in the nesting of A.
end
```
It means A.f looks up the constants at the top level:
```ruby
module A
  B = 42

  def f
    21
  end
end

A.module_eval do
  EVAL_CONST = 100

  def self.f
    p B
  end
end

B = 15

A.f
# => 15

puts "EVAL_CONST is defined? #{A.const_defined?(:EVAL_CONST, false)}" # false
puts "EVAL_CONST is defined? #{Object.const_defined?(:EVAL_CONST, false)}" # true
```

Memo:
If not use const_defined? with false, it will check parent class's constants.
```ruby
puts "EVAL_CONST is defined? #{A.const_defined?(:EVAL_CONST)}" # true
puts "EVAL_CONST is defined? #{Object.const_defined?(:EVAL_CONST)}" # true
```

- If you define a constant at the top level, Object becomes a constant.
```ruby
B = "Hello, world"
p Object.const_get(:B) # "Hello, world
```

**Use a string when you need to access the class's scope, use a block for complex code.**

### instance_eval
Evaluate code in the context of an instance:
```ruby
class User
  def initialize(name)
    @name = name
  end
end

user = User.new("Alice")
user.instance_eval do
  @age = 25
  def custom_method
    "#{@name} is #{@age} years old"
  end
end

user.custom_method # => "Alice is 25 years old"
```

## Introspection and Reflection

### Instance Variable Access
```ruby
class User
  def initialize(name, email)
    @name = name
    @email = email
  end
end

user = User.new("Alice", "alice@example.com")

# Get instance variable
user.instance_variable_get(:@name) # => "Alice"

# Set instance variable
user.instance_variable_set(:@age, 30)

# List all instance variables
user.instance_variables # => [:@name, :@email, :@age]

# Check if instance variable exists
user.instance_variable_defined?(:@name) # => true
```

### Method Introspection
```ruby
class User
  def public_method; end
  
  private
  def private_method; end
  
  protected
  def protected_method; end
end

user = User.new

# Check method existence
user.respond_to?(:public_method) # => true
user.respond_to?(:private_method, true) # => true (include private)

# List methods
User.instance_methods(false) # => [:public_method]
User.private_instance_methods(false) # => [:private_method]
User.protected_instance_methods(false) # => [:protected_method]

# Get method object
method_obj = user.method(:public_method)
method_obj.call
```

### Constant Access
```ruby
class User
  VERSION = "1.0"
end

# Get constant
User.const_get("VERSION") # => "1.0"
Object.const_get("User::VERSION") # => "1.0"

# Set constant
User.const_set("API_VERSION", "v2")
User::API_VERSION # => "v2"

# List constants
User.constants # => [:VERSION, :API_VERSION]

# Check constant existence
User.const_defined?("VERSION") # => true
```

## Class and Module Manipulation

### Dynamic Class Creation
```ruby
# Create class dynamically
MyDynamicClass = Class.new do
  def initialize(name)
    @name = name
  end
  
  def greet
    "Hello, I'm #{@name}"
  end
end

obj = MyDynamicClass.new("Dynamic")
obj.greet # => "Hello, I'm Dynamic"
```

### Dynamic Module Creation
```ruby
# Create module dynamically
MyDynamicModule = Module.new do
  def self.included(base)
    base.extend(ClassMethods)
  end
  
  module ClassMethods
    def create_with_name(name)
      new.tap { |obj| obj.instance_variable_set(:@name, name) }
    end
  end
  
  def name
    @name
  end
end

class User
  include MyDynamicModule
end

user = User.create_with_name("Alice")
user.name # => "Alice"
```

### Class Modification at Runtime
```ruby
class User; end

# Add methods to existing class
User.class_eval do
  attr_accessor :name
  
  def greet
    "Hello, #{@name}"
  end
end

# Add class methods
User.singleton_class.class_eval do
  def create_admin(name)
    new.tap { |user| user.name = "Admin #{name}" }
  end
end

admin = User.create_admin("Bob")
admin.greet # => "Hello, Admin Bob"
```

## Advanced Metaprogramming Techniques

### Method Hooks
```ruby
class User
  def self.method_added(method_name)
    puts "Method #{method_name} was added to #{self}"
  end
  
  def greet
    "Hello!"
  end
end
# Output: "Method greet was added to User"
```

### Singleton Methods
```ruby
user = User.new

# Define singleton method
def user.special_greeting
  "I'm special!"
end

# Or using define_singleton_method
user.define_singleton_method(:another_greeting) do
  "Another special greeting!"
end

user.special_greeting # => "I'm special!"
user.another_greeting # => "Another special greeting!"
```

### Method Delegation
```ruby
class UserProfile
  def initialize(user)
    @user = user
  end
  
  # Delegate methods to @user
  [:name, :email, :age].each do |method|
    define_method(method) do |*args, &block|
      @user.send(method, *args, &block)
    end
  end
end
```

### Creating DSLs (Domain Specific Languages)
```ruby
class ConfigBuilder
  def initialize
    @config = {}
  end
  
  def method_missing(name, *args)
    if args.length == 1
      @config[name] = args.first
    elsif args.empty?
      @config[name]
    else
      super
    end
  end
  
  def build
    @config
  end
end

def configure(&block)
  builder = ConfigBuilder.new
  builder.instance_eval(&block)
  builder.build
end

config = configure do
  host "localhost"
  port 3000
  ssl true
end

config # => {:host=>"localhost", :port=>3000, :ssl=>true}
```

## Best Practices and Warnings

### Security Considerations
- **Never use `eval` with user input** - it can lead to code injection
- **Validate method names** when using `send` dynamically
- **Use `public_send`** instead of `send` when possible

### Performance Considerations
- **Metaprogramming can be slower** than regular method calls
- **Use metaprogramming judiciously** - prefer explicit code when performance matters
- **Cache method objects** when calling them repeatedly

### Debugging and Maintainability
- **Document metaprogrammed code thoroughly**
- **Use meaningful method names** even when generated dynamically
- **Prefer convention over metaprogramming** when possible
- **Keep metaprogramming localized** to specific areas of your codebase

### Example: Well-designed Metaprogramming
```ruby
module Trackable
  def self.included(base)
    base.extend(ClassMethods)
  end
  
  module ClassMethods
    def track_method(method_name, event_name = nil)
      event_name ||= "#{name.downcase}_#{method_name}"
      
      alias_method "#{method_name}_without_tracking", method_name
      
      define_method(method_name) do |*args, &block|
        result = send("#{method_name}_without_tracking", *args, &block)
        track_event(event_name, { method: method_name, args: args })
        result
      end
    end
  end
  
  private
  
  def track_event(event_name, data)
    puts "Tracked: #{event_name} with #{data}"
  end
end

class User
  include Trackable
  
  def login
    "User logged in"
  end
  
  track_method :login
end

user = User.new
user.login # => "User logged in" and tracks the event
``` 