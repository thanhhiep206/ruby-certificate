# Object-Oriented Programming: Inheritance, Modules and Mixins

## Class Inheritance

### Basics of Inheritance
Ruby follows a single inheritance model where a class can inherit from only one parent class:
```ruby
class Parent
  def greet
    "Hello from Parent"
  end
end

class Child < Parent
end

puts Child.new.greet # => "Hello from Parent"
```

### Method Overriding
Subclasses can override methods from their superclass:
```ruby
class Parent
  def greet
    "Hello from Parent"
  end
end

class Child < Parent
  def greet
    "Hello from Child"
  end
end

puts Child.new.greet # => "Hello from Child"
```

### Using super
Access parent's implementation using `super`:
```ruby
class Parent
  def greet
    "Hello from Parent"
  end
end

class Child < Parent
  def greet
    "#{super}, and Child"
  end
end

puts Child.new.greet # => "Hello from Parent, and Child"
```

### Inheritance Chain and Method Lookup Path
Ruby resolves methods using the Method Lookup Path:
```ruby
module MyModule
  def my_method
    puts "MyModule"
  end
end

module MyModule2
  def my_method
    puts "MyModule2"
  end
end

class ParentClass
  def my_method
    puts "ParentClass"
  end
end

module PrependModule1
  def my_method
    puts "PrependModule"
  end
end

module PrependModule2
  def my_method
    puts "PrependModule2"
  end
end

module PrependModule3
  def my_method
    puts "PrependModule3"
  end
end

module MyModule1
  def my_method
    puts "MyModule1"
  end
end

module MyModule
  include MyModule1
end

class MyClass < ParentClass
  include MyModule
  include MyModule2
  prepend PrependModule1, PrependModule2
  prepend PrependModule3
end

MyClass.ancestors 
# => [PrependModule3, PrependModule1, PrependModule2, MyClass, MyModule2, MyModule, MyModule1, ParentClass, Object, Kernel, BasicObject]
MyClass.new.my_method # => "PrependModule3"
```

### Accessing Constants in Nested Modules
```ruby
module A
  p Module.nesting  # [A]
  
  module B
    p Module.nesting  # [A::B, A]
  end

  module B::C
    p Module.nesting  # [A::B::C, A]
  end
end

module A::D
  p Module.nesting  # [A::D]
end

module M1
  class C1
    p Module.nesting  # [M1::C1, M1]
    CONST = "001"
  end

  class C2 < C1
    p Module.nesting  # [M1::C2, M1]
    CONST = "010"

    module M2
      p Module.nesting  # [M1::C2::M2, M1::C2, M1]
      CONST = "011" # M2::CONST

      class Ca
        p Module.nesting  # [M1::C2::M2::Ca, M1::C2::M2, M1::C2, M1]
        CONST = "100" # Ca::CONST
      end

      class Cb < Ca
        p Module.nesting  # [M1::C2::M2::Cb, M1::C2::M2, M1::C2, M1]
        p CONST # 011
      end
    end
  end
end
```
When module D is defined, the nesting does not include module A. Hence, module D cannot access the constants defined in module A, without explicitly using the scope resolution (::) operator.

```ruby
class Human
  NAME = "Unknown"

  def name
    NAME
  end
end

class Noguchi < Human
  NAME = "Hideyo"
end

puts Noguchi.new.name # => "Unknown"
```
Const Name is in Human class, method name is define in Human class.
Ruby find Name in Lexical scope – class Human.

### Object Lifecycle and Initialization
Subclasses can override initialize, but should use super when needed:
```ruby
class Parent
  def initialize(name)
    @name = name
  end
end

class Child < Parent
  def initialize(name, age)
    super(name)
    @age = age
  end
end
```

### Polymorphism
Dynamic method resolution at runtime:
```ruby
class Animal
  def speak
    "..."
  end
end

class Dog < Animal
  def speak
    "Woof!"
  end
end

class Cat < Animal
  def speak
    "Meow!"
  end
end

[Dog.new, Cat.new].each { |animal| puts animal.speak }
# => "Woof!"
# => "Meow!"
```

### Class Relationship Checks
```ruby
class Parent; end
class Child < Parent; end

child = Child.new
puts child.is_a?(Parent)      # => true
puts child.instance_of?(Parent) # => false
puts child.class == Child     # => true
```

### Global Scope Access
Use `::` to access classes in global scope:
```ruby
class C
  CONST = "constant"
end

module M
  class ::C
    p CONST  # => "constant"
  end
end
```

## Modules and Mixins

### What Are Modules?
Modules are collections of methods, constants, and class variables that provide:
- **Mixins**: Shared behavior across classes
- **Namespaces**: Organization and avoiding naming collisions
- **No Instantiation**: Cannot be instantiated like classes
- **No Inheritance**: Cannot be subclassed

### Core Module Concepts

#### include vs extend
```ruby
module Greetable
  def greet
    "Hello!"
  end
end

class User
  include Greetable  # Adds as instance methods
end

class Admin
  extend Greetable   # Adds as class methods
end

User.new.greet     # => "Hello!"
Admin.greet        # => "Hello!"
```

```ruby
module M
  extend self
  def a
    100
  end
end

p M.a # => 100

# module_function is a method to make a method a module function
module M
  def a
    100
  end

  module_function :a
end

p M.a # => 100

# class << self is a method to make a method a class function
module M
  class << self
    def a
      100
    end
  end
end

p M.a # => 100
```

#### Module as Namespace
```ruby
module PaymentGateway
  class Transaction
    def process
      "Processing payment"
    end
  end
  
  API_VERSION = "v2"
end

transaction = PaymentGateway::Transaction.new
puts PaymentGateway::API_VERSION
```

#### Module Constants
```ruby
module MathUtils
  PI = 3.14159
  E = 2.71828
  
  def self.circle_area(radius)
    PI * radius * radius
  end
end

puts MathUtils::PI  # => 3.14159
puts MathUtils.circle_area(5)
```

### Advanced Module Features

#### prepend
Places module before class in method lookup chain:
```ruby
module Greetings
  def hello
    "Hello from Module, " + super
  end
end

class User
  prepend Greetings
  
  def hello
    "Hello from User"
  end
end

puts User.new.hello  # => "Hello from Module, Hello from User"
```

#### Comparison: include vs prepend

| Aspect              | include                                | prepend                                |
|---------------------|----------------------------------------|----------------------------------------|
| Method Precedence   | Class methods override module methods  | Module methods override class methods  |
| Method Lookup Chain | Module appears after the class        | Module appears before the class       |
| Primary Use         | Add shared behavior to classes         | Override or extend behavior            |

#### Module Hooks
Customize behavior when modules are included or extended:
```ruby
module Trackable
  def self.included(base)
    puts "#{base} included #{self}"
    base.extend(ClassMethods)
  end
  
  def self.extended(base)
    puts "#{base} extended #{self}"
  end
  
  module ClassMethods
    def track_changes
      @track_changes = true
    end
  end
  
  def log_change
    puts "Change logged" if self.class.instance_variable_get(:@track_changes)
  end
end

class User
  include Trackable  # => "User included Trackable"
  track_changes
end
```

#### Module extend self
Make module methods available as both instance and class methods:
```ruby
module Parent
  def method_1
    __method__ # Return symbol of the method name :method_1
  end
end

module Child
  include Parent
  extend self
end

Child.method_1  # => :method_1
```

#### Dynamic Module Inclusion
Include modules at runtime:
```ruby
module Loggable
  def log(message)
    puts "[LOG] #{message}"
  end
end

class DynamicClass
  def self.add_logging
    include Loggable
  end
end

DynamicClass.add_logging
DynamicClass.new.log("Hello") # => "[LOG] Hello"
```

#### Module Nesting
```ruby
module SuperMod
  module BaseMod
    p Module.nesting  # => [SuperMod::BaseMod, SuperMod]
    
    class InnerClass
      p Module.nesting  # => [SuperMod::BaseMod::InnerClass, SuperMod::BaseMod, SuperMod]
    end
  end
end
```

### Reopening Modules
Modules can be reopened to add or modify methods:
```ruby
module M1
  def method_1
    "Hello from M1"
  end
end

class MyClass
  include M1
end

# Reopen M1 to add more functionality
module M1
  def method_2
    "Hello from M2"
  end
end

obj = MyClass.new
obj.method_1  # => "Hello from M1"
obj.method_2  # => "Hello from M2"
```

## Important Ruby Standard Modules

### Enumerable
Adds traversal, searching, and sorting to collections:
```ruby
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

list = TodoList.new
list << "Buy milk"
list << "Walk dog"
list.map(&:upcase)  # => ["BUY MILK", "WALK DOG"]
```

### Comparable
Adds comparison methods:
```ruby
class Version
  include Comparable
  
  attr_reader :major, :minor, :patch
  
  def initialize(version_string)
    @major, @minor, @patch = version_string.split('.').map(&:to_i)
  end
  
  def <=>(other)
    [major, minor, patch] <=> [other.major, other.minor, other.patch]
  end
end

v1 = Version.new("2.1.0")
v2 = Version.new("2.0.5")
v1 > v2  # => true
```

### Math
Provides mathematical methods and constants:
```ruby
Math::PI      # => 3.141592653589793
Math.sqrt(16) # => 4.0
Math.sin(Math::PI / 2)  # => 1.0
```

## Best Practices

### Inheritance Best Practices
- **Favor composition over inheritance** for shared behavior when possible
- **Avoid deep inheritance hierarchies**; prefer flat structures
- **Use abstract classes** to define shared behavior while leaving specific implementation to subclasses
- **Document parent classes** well to clarify their intended usage

### Module Best Practices
- **Single Responsibility**: Design modules to handle one well-defined responsibility
- **DRY Principle**: Use modules to remove code duplication
- **Meaningful Names**: Choose descriptive names that indicate the module's purpose
- **Keep modules cohesive**: Related functionality should be grouped together

### Example: Well-designed Module System
```ruby
module Authenticatable
  def self.included(base)
    base.extend(ClassMethods)
  end
  
  module ClassMethods
    def authenticate(email, password)
      # Authentication logic
    end
  end
  
  def authenticated?
    !@auth_token.nil?
  end
  
  def logout
    @auth_token = nil
  end
end

module Trackable
  def track_event(event_name, data = {})
    # Event tracking logic
  end
end

class User
  include Authenticatable
  include Trackable
  
  def initialize(email)
    @email = email
  end
end
``` 
