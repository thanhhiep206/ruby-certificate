### What Are Modules in Ruby?
- Modules are collections of methods, constants, and class variables that can be included or extended in classes to share functionality.
- They enable mixins, namespace organization, and separation of concerns without inheritance.

### Modules vs. Classes
- No Instantiation: Modules cannot be instantiated like classes.
- No Subclassing: Modules cannot be subclassed.
- Use in Composition: They are used to share behavior across unrelated classes.

### Core Concepts of Modules
#### Mixins
- Achieved using include or extend.
- include: Adds methods from the module as instance methods in the target class.
- extend: Adds methods from the module as class methods in the target class.
```ruby
class MyClass
  include MyModule1
  extend MyModule2
end
```

- In module, extend self will make the module methods as class methods.
```ruby
module Parent
  def method_1
    __method__
  end
end

module Child
  include Parent
  extend self
end

p Child::method_1 # => "method_1"
```

- If later include a module and update the inheritance relationship, the method will not be found.
```ruby
module Parent
  def method_1
    __method__
  end
end

module Child
  extend self
  include Parent
end

p Child::method_1 # => undefined method
```

#### Namespaces
- Modules can encapsulate methods, constants, or classes to avoid naming collisions.
```ruby
module PaymentGateway
  class Transaction
    # Transaction logic
  end
end
```

#### Constants
- Modules can define constants which are accessible using the scope operator ::
```ruby
module MathUtils
  PI = 3.14159
end

puts MathUtils::PI  # Output: 3.14159
```

#### Modules as Libraries
- Ruby standard libraries like Math or Enumerable are implemented as modules.
- They can be included or extended in your classes.

### Common Use Cases of Modules
- Shared Behavior via Mixins
- Namespace Management
- Adding Class Methods
```ruby
module ClassUtilities
  def find_by_id(id)
    # Logic to find by ID
  end
end

class Product
  extend ClassUtilities
end

Product.find_by_id(1)
```

### Advanced Module Features
#### prepend
- **prepend** is used to include a module into a class but places the module before the class in the method lookup chain.
- This allows methods in the module to override or extend methods in the class more cleanly compared to include
```ruby
module Greetings
  def hello
    "Hello from Module"
  end
end

class User
  prepend Greetings
  
  def hello
    "Hello from User"
  end
end

puts User.new.hello  # Output: Hello from Module
```

- using super
```ruby
module A
  def greet
    "Hello from A, " + super
  end
end

class B
  prepend A

  def greet
    "Hello from B"
  end
end

puts B.new.greet
# Output: "Hello from A, Hello from B"
```

| Aspect              | include                                | prepend                                |
|---------------------|----------------------------------------|----------------------------------------|
| Method Precedence	  | Class methods override module methods. | Module methods override class methods. |
| Method Lookup Chain | Module appears after the class.	       | Module appears before the class.       |
| Primary Use	        | Add shared behavior to classes.	       | Override or extend behavior.           |

#### Hooks in Modules
- included and extended hooks allow modules to customize their behavior when included or extended.
```ruby
module Trackable
  def self.included(base)
    puts "#{base} included #{self}"
  end
end

class User
  include Trackable
end
```

- Accessing Original Method
```ruby
module A
  def greet
    "Hello from A"
  end
end

class B
  prepend A
  alias_method :original_greet, :greet

  def greet
    "Hello from B, " + original_greet
  end
end

puts B.new.greet
# Output: "Hello from B, Hello from A"
```

#### Module Functions
- Module methods can be used as both instance methods and standalone functions.
```ruby
module MathUtils
  def self.square(x)
    x * x
  end
end

puts MathUtils.square(4)  # Output: 16
```

#### Dynamic Inclusion
- Use include or extend dynamically at runtime.
```ruby
class DynamicInclude
  def self.add_logging
    include Loggable
  end
end
```

#### nesting
- Modules can be nested within other modules.
```ruby
module SuperMod
  module BaseMod
    p Module.nesting
  end
end

# => [SuperMod::BaseMod, SuperMod]
```

### Important Ruby Standard Modules
- Enumerable: Adds traversal, searching, and sorting to collections.
- Comparable: Adds comparison methods (<, <=, ==, >=, >).
- Math: Provides mathematical methods and constants.

### Best Practices
- Single Responsibility: Design modules to handle a single, well-defined responsibility.
- DRY Principle: Use modules to remove duplication.
- Avoid Overloading: Limit the use of multiple modules in a single class to maintain clarity.
- Document Your Modules: Clearly describe their purpose and usage.
