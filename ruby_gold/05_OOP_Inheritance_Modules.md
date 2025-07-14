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

### Accessing Constants in Nested Modules (Module Nesting)
```ruby
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
=> When module D is defined, the nesting does not include module A. Hence, module D cannot access the constants defined in module A, without explicitly using the scope resolution (::) operator.

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
=> Const Name is in Human class, method name is define in Human class.
=> Ruby find Name in Lexical scope – class Human.

### Class Relationship Checks
```ruby
class Parent; end
class Child < Parent; end

child = Child.new
puts child.is_a?(Parent)      # => true
puts child.instance_of?(Parent) # => false
puts child.class == Child     # => true
puts child.class.class == Class # => true
puts Class.superclass == Module # => true
puts Parent.superclass # => Module
puts Child.superclass # => Parent
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
