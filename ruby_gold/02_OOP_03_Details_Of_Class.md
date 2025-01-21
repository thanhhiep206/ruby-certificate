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

### Singleton Methods and Singleton Classes
See [Singleton Methods and Singleton Classes](./02_OOP_02_Access_Control.md)

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

