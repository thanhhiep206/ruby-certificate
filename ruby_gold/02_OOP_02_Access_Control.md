### Understanding Access Control in Ruby
- Access control determines which methods are accessible to the outside world and which are private to the class or object.
- Encapsulation and maintaining control over object behavior.

### Types of Access Control
#### Public Methods
- Accessible from anywhere, both inside and outside the class.
- Default access level unless otherwise specified.
- Use cases:
  - Methods intended for external interaction (API methods).
```ruby
class MyClass
  def public_method
    # method body
  end
end
```

#### Protected Methods
- Can only be called by instances of the defining class or its subclasses.
- Not directly accessible outside the class hierarchy.
- Use cases:
  - Sharing functionality between instances of the same class or subclasses.
```ruby
class MyClass
  protected
    
  def protected_method
    "I'm protected!"
  end
end
```

#### Private Methods
- Can only be called within the context of the defining class.
- Not accessible by any other instance, even of the same class.
- Use cases:
  - Internal implementation details that should not be exposed.
```ruby
class MyClass
  private

  def private_method
    "I'm private!"
  end
end
```

### Defining Access Levels
- Use public, protected, and private keywords to define access levels.
- Group methods by their access level for clarity.
```ruby
class MyClass
  def initialize
    @value = 42
  end

  public
  def public_method
    "Public: #{@value}"
  end

  protected
  def protected_method
    "Protected: #{@value}"
  end

  private
  def private_method
    "Private: #{@value}"
  end
end
```

### Using private and protected with Arguments
- Ruby allows passing method names as symbols to private and protected:
```ruby
class MyClass
  class MyClass
  def method_one; end
  def method_two; end

  private :method_one
  protected :method_two
end
```

### Access Control in Singleton Methods
- Singleton methods also support access control:
```ruby
class MyClass
  class << self
    private
    def private_class_method
      "I'm private to the class!"
    end
  end
end
```

### Exceptions to Private Access
- Private methods can be accessed via send or instance_eval, but use this cautiously:
```ruby
class MyClass
  private
  def private_method
    "Private!"
  end
end

obj = MyClass.new
obj.send(:private_method) # => "Private!"

```

### Visibility and Ruby's attr_* Methods
- By default, attr_accessor, attr_reader, and attr_writer define public methods.
- Change access levels by specifying their scope:
```ruby
class MyClass
  attr_reader :value
  private :value
end
```

### Practical Use Cases and Best Practices
- Encapsulation: Limit access to sensitive logic using private methods.
- API Design: Expose only public methods that clients need to interact with.
- Collaboration: Use protected methods for interaction within a class hierarchy.
- Code Clarity: Organize methods by access level and document their purpose.

### Advanced Topics
#### Metaprogramming
- Dynamically define private or protected methods
```ruby
class MyClass
  private
  define_method(:dynamic_private_method) do
    "I'm dynamically private!"
  end
end
```

#### Modules
- Understand how access control works with include and extend.
```ruby
module MyModule
  def public_method
    "Public in module!"
  end
end

# include
class MyClass
  include MyModule
end

obj = MyClass.new
obj.public_method # => "Public in module!"

# extend
class MyClass
  extend MyModule
end

MyClass.public_method  # => "Public in module!"
```
