### Understand the Basics
- Define lambdas using -> or lambda syntax
- The ways to call a lambda using .call or []

```ruby
my_lambda = ->(x) { x * 2 }
puts my_lambda.call(10)  # Outputs: 20
puts my_lambda[10]      # Outputs: 20
```

### Use Cases for Lambdas
- Closures
- Functional Programming
- Code Reusability
```ruby
add = ->(x, y) { x + y }
puts add.call(5, 3)  # Outputs: 8

[1, 2, 3].map(&add.curry[10])  # Outputs: [11, 12, 13]
```

### Advanced Topics
- Currying
Currying is a technique where a function is transformed into a sequence of functions, each with a single argument.
```ruby
add = ->(x, y) { x + y }
add_10 = add.curry[10]
puts add_10.call(5)  # Outputs: 15
```

- Dynamic Dispatch
Dynamic dispatch is a mechanism where the method to be called is determined at runtime, based on the object's class or type.
```ruby
class Animal; end
class Dog < Animal; end
class Cat < Animal; end

animal = Dog.new
animal.speak  # Outputs: "Woof!"
```

- Lambda cannot call with missing arguments.
```ruby
my_lambda = ->(x, y) { x * y }
my_lambda.call(10, 20)  # Outputs: 200
my_lambda.call(10)  # # wrong number of arguments (given 1, expected 2)
```
