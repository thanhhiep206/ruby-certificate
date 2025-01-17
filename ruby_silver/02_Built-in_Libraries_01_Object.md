Object is the default root of all Ruby objects
Object inherits from BasicObject which allows creating alternate object hierarchies
Methods on Object are available to all classes unless explicitly overridden.

Class Object:
- Inherits from class BasicObject.
- Includes module Kernel.

### Querying

#### kind_of?
Returns whether given argument is an ancestor of the singleton class of self.
```ruby
str = "Hello, world!"
puts str.kind_of?(String)  # => true
puts str.kind_of?(Object)  # => true
puts str.kind_of?(Array)   # => false
```

#### instance_of?
Returns whether self is an instance of the given class.
```ruby
class Animal; end
class Dog < Animal; end

dog = Dog.new

puts dog.instance_of?(Dog)      # true
puts dog.instance_of?(Animal)   # false
```

#### instance_variable_defined?
Returns whether the given instance variable is defined in self.

### Other
#### clone
Returns a shallow copy of self, including singleton class and frozen state.

```ruby
original = { key: 'value' }
copy = original.clone

puts copy # {:key=>"value"}
puts original.equal?(copy) # false (chúng là hai object khác nhau)
puts copy.frozen? # true
```

#### dup
Returns a shallow unfrozen copy of self.

```ruby
original = { key: 'value' }
copy = original.dup

puts copy # {:key=>"value"}
puts original.equal?(copy) # false
puts copy.frozen?   # false
```

#### enum_for (aliased as to_enum)
Returns an Enumerator for self using the using the given method, arguments, and block.

```ruby
array = [10, 20, 30]

enumerator = array.to_enum(:each_with_index)

enumerator.each do |value, index|
  puts "Value: #{value}, Index: #{index}"
end
# Output:
# Value: 10, Index: 0
# Value: 20, Index: 1
# Value: 30, Index: 2
```

#### freeze
Prevents further modifications to obj. A FrozenError will be raised if modification is attempted. There is no way to unfreeze a frozen object. See also Object#frozen?.

```ruby
a = [ "a", "b", "c" ]
a.freeze
a << "z"
# => FrozenError (can't modify frozen Array)
```
