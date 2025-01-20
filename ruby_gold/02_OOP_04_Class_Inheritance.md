### Basics of Inheritance
#### Understand how inheritance works in Ruby.
```ruby
class Parent
  def greet
    "Hello from Parent"
  end
end

class Child < Parent
end

puts Child.new.greet # Output: "Hello from Parent"
```

#### Know the single inheritance model in Ruby.
- A Ruby class can inherit from only one parent class (no multiple inheritance).
- Use modules for shared behavior (via mixins) to emulate multiple inheritance.

### Method Overriding
- Understand how a subclass can override methods from its superclass.

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

puts Child.new.greet # Output: "Hello from Child"
```

- Using super to invoke the parent’s implementation.
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

puts Child.new.greet # Output: "Hello from Parent, and Child"
```

### Inheritance Chain
- Ruby resolves methods using the Method Lookup Path.
- Use ancestors to inspect the lookup path.

```ruby
module MyModule
  def my_method
    puts "MyModule"
  end
end

class ParentClass
  def my_method
    puts "ParentClass"
  end
end

module PrependModule
  def my_method
    puts "PrependModule"
  end
end

class MyClass < ParentClass
  include MyModule
  prepend PrependModule
end

MyClass.ancestors # => [PrependModule, MyClass, MyModule, ParentClass, Object, Kernel, BasicObject]
MyClass.new.my_method # => "PrependModule"
```

### Protected and Private Methods
- Private methods are not accessible directly by subclasses.
- Protected methods can be accessed by instances of the same class or subclass.
```ruby
class Parent
  private

  def secret
    "This is a secret"
  end
end

class Child < Parent
  def reveal
    secret # Raises an error
  end
end
```

### Abstract Classes
- Use base classes to define shared behavior for subclasses while leaving specific implementation to them.
```ruby
class Animal
  def speak
    raise NotImplementedError, "Subclasses must implement the speak method"
  end
end

class Dog < Animal
  def speak
    "Woof!"
  end
end
```

### Modules and Inheritance
- Modules can be included or extended for shared behavior without class inheritance.
- The method lookup path includes modules in the order they are included.
```ruby
module Speakable
  def speak
    "Speaking"
  end
end

class Human
  include Speakable
end
```

### Object Lifecycle and Initialization
- Subclasses can override initialize, but super should be used to call the parent’s initialize when needed.
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
- Methods defined in a superclass can be overridden in a subclass, allowing dynamic method resolution at runtime.
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
```

### Best Practices
- Use inheritance judiciously. Favor composition over inheritance for shared behavior when possible.
- Avoid deep inheritance hierarchies; prefer flat structures.
- Use final or freeze patterns to prevent certain methods or classes from being overridden if needed.
- Document your parent classes well to clarify their intended usage by subclasses.

### Ruby-Specific Features
- Use Object#is_a? and Object#instance_of? to check class relationships.
```ruby
class Parent; end
class Child < Parent; end

child = Child.new
puts child.is_a?(Parent)      # true
puts child.instance_of?(Parent) # false
```

- Explore BasicObject and how it acts as the ultimate superclass.
```ruby
class MyClass < BasicObject; end
```
