# Object-Oriented Programming: Access Control and Classes

## Access Control

### Understanding Access Control in Ruby
Access control determines which methods are accessible to the outside world and which are private to the class or object. It provides encapsulation and helps maintain control over object behavior.

### Types of Access Control

#### Public Methods
Default access level - accessible from anywhere:
```ruby
class User
  def public_method
    "I'm accessible from anywhere!"
  end
end

user = User.new
user.public_method  # Works fine
```

#### Private Methods
Only callable within the defining class context:
```ruby
class BankAccount
  def initialize(balance)
    @balance = balance
  end
  
  def withdraw(amount)
    return "Insufficient funds" unless sufficient_funds?(amount)
    @balance -= amount
  end
  
  private
  
  def sufficient_funds?(amount)
    @balance >= amount
  end
end

account = BankAccount.new(100)
account.withdraw(50)  # Works
# account.sufficient_funds?(25)  # NoMethodError
```

**Note**: `initialize` is always private by default.

#### Protected Methods
Accessible by instances of the same class or subclasses:
```ruby
class Person
  def initialize(age)
    @age = age
  end
  
  def older_than?(other_person)
    @age > other_person.age  # Can access protected method of other instance
  end
  
  protected
  
  def age
    @age
  end
end

alice = Person.new(30)
bob = Person.new(25)
alice.older_than?(bob)  # => true
# alice.age  # NoMethodError (protected method)
```

## Class Details

### Basic Class Definition
```ruby
class User
  attr_accessor :name, :email

  def initialize(name, email)
    @name = name
    @email = email
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

#### Instance Variables (@variable)
```ruby
class User
  def initialize(name)
    @name = name  # Instance variable
  end
  
  def name
    @name
  end
end
```

#### Object Encapsulation
```ruby
class BankAccount
  def initialize(balance)
    @balance = balance
  end

  def deposit(amount)
    @balance += amount
  end

  def withdraw(amount)
    @balance -= amount if @balance >= amount
  end

  def display_balance
    "Current balance: $#{@balance}"
  end
end
```

### Class Variables and Class Methods

#### Class Variables (@@variable)
Shared data across all instances:
```ruby
class Counter
  @@count = 0

  def self.increment
    @@count += 1
  end

  def self.current_count
    @@count
  end
  
  def initialize
    @@count += 1
  end
end

Counter.increment
puts Counter.current_count # => 1
```

- With singleton class, Ruby will find class variable by lexical scope
```ruby
class C
  @@val = 10
end

module B
  @@val = 30
end

module M
  include B
  @@val = 20

  class << C
    p @@val
  end
end

# => 20
```

#### Class Methods (self.method_name)
```ruby
class User
  def self.create_admin(name)
    new(name, admin: true)
  end
end
```

### Singleton Methods and Classes

#### Singleton Methods
Methods defined for a specific object:
```ruby
user = User.new("Alice")
def user.special_greeting
  "I'm special: #{@name}"
end

user.special_greeting  # => "I'm special: Alice"
```

#### Singleton Classes
```ruby
class MyClass
  def my_method
    puts "MyClass"
  end
end

MyClass.singleton_class.define_method(:my_method) { puts "SingletonClass" }
MyClass.my_method # => "SingletonClass"

# Memo: self in singleton class is MyClass.

# Alternative syntax
class << MyClass
  def class_method
    "I'm a class method"
  end
end
```

- Singleton scope
```ruby
class C
  CONST = "Hello, world"
end

$c = C.new

class D
  class << $c
    def say
      CONST
    end
  end
end

# [#<Class:#<C:0x007fa4741607e0>>, C, Object, Kernel, BasicObject]
p $c.say
# => Hello, world
```

- self in singleton class is the class itself
```ruby
class C
end

class << C
  self # => C
end

# can call singleton class method
p C.singleton_class # => #<Class:#<C:0x007fa4741607e0>>
```

### attr_* Methods

#### Basic Usage
```ruby
class User
  attr_reader :name      # Read-only
  attr_writer :email     # Write-only  
  attr_accessor :age     # Read and write
end
```

#### Access Control with attr_*
```ruby
class MyClass
  attr_reader :value
  private :value  # Make reader private
end
```

#### Important Note
attr_* cannot use super:
```ruby
class MyClass
  attr_accessor :name

  def name
    "Mr. #{super}"  # This will cause an error
  end
end
```

### Advanced Class Features

#### Object Introspection
```ruby
class MyClass
  def inspect
    "MyClass instance"
  end
end

p MyClass.new # => "MyClass instance"

# Default inspect shows instance variables
class User
  def initialize(name)
    @name = name
  end
end

p User.new("Alice")         # => #<User:0x... @name="Alice">
p User.new("Alice").inspect # => "#<User:0x00007fffe381f318 @name=\"Alice\">"
puts User.new("Alice")      # => #<User:0x00007fffe3699ed0>
puts User.new("Alice").to_s # => #<User:0x00007fffe3694c50>
print User.new("Alice")     # => #<User:0x00007fffe366a888>
puts User.new("Alice").inspect # => #<User:0x00007fffe3675c10 @name="Alice">
```

#### Freezing Objects
- Cannot action destructive methods on frozen objects
```ruby
str = "Hello"
str.freeze
str.upcase!  # => FrozenError: can't modify frozen String
```

- Can set value to frozen objects
```ruby
hoge = "hoge".freeze
hoge = "foo".freeze
puts hoge # => "foo"
```

#### Understanding self in Different Contexts
```ruby
self # => main

class MyClass
  self # => MyClass
  
  def instance_method
    self # => instance of MyClass
  end
  
  def self.class_method
    self # => MyClass
  end
end
```
