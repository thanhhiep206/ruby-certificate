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
