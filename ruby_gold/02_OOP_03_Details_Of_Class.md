### Basics of Classes in Ruby
```ruby
class User
  attr_accessor :name, :email

  def initialize(name, email)
    @name = name
    @name = email
  end

  def greet
    "Hello, #{@name}!"
  end
end

# Usage
user = User.new("Alice", "alice@example.com")
puts user.greet
```

### Instance Variables and Methods
#### Instance variables (@variable) and their scope.
```ruby
class User
  @name = "Alice"
end

User.name # => "Alice"
```

#### Defining instance methods.
```ruby
class User
  def greet
    "Hello, #{@name}!"
  end
end
```

#### Object encapsulation.
```ruby
class BankAccount
  def initialize(balance)
    @balance = balance
  end

  def deposit(amount)
    @balance += amount
  end

  def withdraw(amount)
    @balance -= amount
  end

  def display_balance
    "Current balance: $#{@balance}"
  end
end
```

### Class Variables and Class Methods
- Class variables (@@variable) for shared data across all instances.
- Class methods (self.method_name) for actions related to the class.

```ruby
class Counter
  @@count = 0

  def self.increment
    @@count += 1
  end

  def self.current_count
    @@count
  end
end

Counter.increment
puts Counter.current_count # Output: 1
```

### Inheritance and Polymorphism
- Subclasses and inheritance using <.
- Overriding methods.
- super keyword for accessing parent class behavior.

```ruby
class Animal
  def speak
    "I make a sound"
  end
end

class Dog < Animal
  def speak
    "Woof!"
  end
end

dog = Dog.new
puts dog.speak # Output: Woof!
```

### Modules and Mixins
See [Modules and Mixins](./02_OOP_02_Access_Control.md#Modules)

### Access Control: public, protected, private
See [Access Control](./02_OOP_02_Access_Control.md)

### Singleton Methods
See [Singleton Methods](./02_OOP_02_Access_Control.md)

### Singleton Classes
Singleton Classes is a class that is created for a specific object.

```ruby
class MyClass
  def my_method
    puts "MyClass"
  end
end

MyClass.singleton_class.define_method(:my_method) { puts "SingletonClass" }
MyClass.my_method # => "SingletonClass"
```

```ruby
class C
  class << C
    def hoge
      'Hi'
    end
  end
end

p C.hoge # => "Hi"
```

### attr_*
- attr_* cannot be use super.
```ruby
class MyClass
  attr_accessor :name

  def name
    "Mr. #{super}"
  end
end

obj = MyClass.new
obj.name = "John"
p obj.name # => super: no superclass method `name' for an instance of MyClass
```


### Advanced Topics
- ObjectSpace and introspection
ObjectSpace is a module that provides a way to inspect and manipulate the objects in the Ruby runtime.
```ruby
ObjectSpace.each_object(Class) { |klass| puts klass }
```

- self in different contexts
```ruby
self # => main

class MyClass
  self # => MyClass
end
```

- Freezing objects with freeze.
```ruby
str = "Hello"
str.freeze
str.reverse # => "Hello"
```

- inspect
```ruby
class MyClass
  def inspect
    "MyClass"
  end
end

p MyClass.new # => "MyClass"

class MyClass2
end

p MyClass2.new # => #<MyClass2:0x0000000100000000>

class Hoge
  def initialize
    @fizz = "bazz"
  end
end

p Hoge.new # => #<Hoge:0x0000000100000000 @fizz="bazz">
```

### Design Patterns with Classes
- Implementing common design patterns like Singleton, Factory, and Strategy.
#### Singleton
```ruby
class Singleton
  @@instance = nil

  def self.instance
    @@instance ||= new
  end
end
```

#### Factory
```ruby
class Factory
  def self.create(type)
    case type
    when :car
      Car.new
    when :bike
      Bike.new
    end
  end
end
```

#### Strategy
```ruby
class Strategy
  def execute
    # implementation
  end
end
```

