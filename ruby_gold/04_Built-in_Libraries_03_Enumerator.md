Enumerator is a class that allows external iteration over collections, enabling more flexible control of how you traverse or manipulate data.

It separates the iteration logic from the collection, making the code cleaner and more modular.

### Creating Enumerators
- Basic Creation
```ruby
array = [1, 2, 3]
enum = array.each   # Returns an Enumerator
```

- With a Block
```ruby
enum = Enumerator.new do |yielder|
  3.times { |i| yielder << i }
end
enum.to_a  # => [0, 1, 2]
```

### Enumerator Methods
#### Iteration Methods
- each: Invokes the block for each element.
- next: Retrieves the next element, raises StopIteration if no more elements.
```ruby
enum = [10, 20, 30].each
enum.next  # => 10
enum.next  # => 20
```
- rewind: Resets the enumerator to its initial state.

#### Transformation and Filtering
- map, select, reject, etc., return new enumerators
```ruby
enum = (1..5).to_enum
enum.map { |x| x * 2 }.to_a  # => [2, 4, 6, 8, 10]
```

#### Chaining
- Enumerators can be chained for complex operations.
```ruby
enum = (1..Float::INFINITY).lazy.select(&:odd?).map { |x| x**2 }
enum.first(5)  # => [1, 9, 25, 49, 81]
```

### Lazy Enumerators
- Purpose: Delay the evaluation of data until explicitly required, useful for working with infinite sequences or large datasets.
```ruby
lazy_enum = (1..).lazy.map { |x| x * 2 }
lazy_enum.first(10)  # => [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
```

- Common Methods:
  - lazy: Converts an enumerator into a lazy enumerator.
  - Operations like select, reject, map can be performed lazily.

### Enumerator::Generator
- Custom Generators: You can define custom iteration behavior using Enumerator::Generator.
```ruby
gen = Enumerator.new do |y|
  y << 1
  y << 2
  y << 3
end
gen.to_a  # => [1, 2, 3]
```

### Enumerator::Yielder
- Yielder Object: Enumerator::Yielder is used to yield values in custom enumerators.
```ruby
enum = Enumerator.new do |yielder|
  3.times { |i| yielder.yield(i) }
end
enum.to_a  # => [0, 1, 2]
```

### Infinite Enumerators
- Use Float::INFINITY to create infinite ranges.
```ruby
infinite_enum = (1..Float::INFINITY).lazy
infinite_enum.select { |x| x % 5 == 0 }.first(10)  # => [5, 10, 15, 20, 25, 30, 35, 40, 45, 50]
```

###  Enumerator and Fiber
- Advanced Usage: Enumerator internally uses fibers, making it capable of creating co-routines.
```ruby
fiber = Fiber.new do
  Fiber.yield "Hello"
  Fiber.yield "World"
  "Done"
end
fiber.resume  # => "Hello"
fiber.resume  # => "World"
fiber.resume  # => "Done"
```

### Enumerator vs Enumerable
- Enumerable: A mixin that provides traversal and searching capabilities for classes like Array, Hash, etc.
- Enumerator: An object that implements these methods without requiring the object to include Enumerable.

