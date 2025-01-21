A Proc (short for procedure) is an encapsulation of a block of code that can be stored in a variable, passed to a method, or executed. It's a core Ruby feature that enhances flexibility and reusability of code.

### Creating a Proc
- Explicitly using Proc.new
```ruby
my_proc = Proc.new { |x| x * 2 }
puts my_proc.call(5) # Output: 10
```

- Using Kernel's proc method:
```ruby
my_proc = proc { |x| x ** 2 }
puts my_proc.call(3) # Output: 9
```

- Using a lambda (special type of Proc):
```ruby
my_lambda = ->(x) { x + 1 }
puts my_lambda.call(4) # Output: 5
```

### Differences Between Proc and Lambda
| Feature           | Proc                           | Lambda                             |
|-------------------|--------------------------------|------------------------------------|
| Argument Handling	| Allows missing or extra args.	 | Enforces exact number of args.     |
| return Behavior   | Exits from enclosing method.	 | Exits only from the lambda itself. |
| Definition Syntax | Proc.new or proc               | -> or lambda                       |

### Proc Methods
- []: Allows access to the Proc's code object.
```ruby
p = Proc.new { |x| x + 2 }
puts p[5] # Output: 7
```

- call: Executes the block with optional arguments.
```ruby
p = Proc.new { |x| x + 2 }
puts p.call(5) # Output: 7
```

- yield: Allows a Proc to yield control back to the block provided.
```ruby
def use_proc(p)
  p.call
  yield if block_given?
end

use_proc(Proc.new { puts "Hello from Proc" }) { puts "Hello from block" }
```

- arity: Returns the number of arguments a Proc expects.
```ruby
p = ->(x, y) { x + y }
puts p.arity # Output: 2
```

- source_location: Returns the file and line where the Proc was defined.
```ruby
p = Proc.new { puts "Code here" }
puts p.source_location # Output: ["file_path", line_number]
```

- parameters: Lists the parameters accepted by the Proc.
```ruby
p = ->(x, y = 1, *args, z:, **kwargs) { }
puts p.parameters.inspect
# Output: [[:req, :x], [:opt, :y], [:rest, :args], [:keyreq, :z], [:keyrest, :kwargs]]
```

### Use Cases for Procs
- Shadowing variables:
```ruby
foo = Proc.new { |n|
  n * 3
}

def foo(n)
  n ** n
end

puts foo[2] * 2
```
=> The foo method is shadowed by a local variable named foo that references a Proc object.
This means that within this scope, foo now refers to the Proc, not the method

To call the original foo method (if needed), you can use the self keyword:
```ruby
self.foo(2)
```

- Callbacks: Pass a Proc to another method for execution.
```ruby
def execute_callback(callback)
  callback.call
end

p = Proc.new { puts "Callback executed!" }
execute_callback(p)
```

- Custom Iterators:
```ruby
def my_each(array, &block)
  array.each { |element| block.call(element) }
end

my_each([1, 2, 3]) { |x| puts x * 2 }
```

- Dynamic Method Definitions:
```ruby
define_method(:add) { |a, b| a + b }
puts add(2, 3) # Output: 5
```

- Functional Programming:
```ruby
add = Proc.new { |x, y| x + y }
multiply = ->(x, y) { x * y }

puts add.call(2, 3) # Output: 5
puts multiply.call(2, 3) # Output: 6
```

- Configuration or Initialization:
```ruby
setup = Proc.new { puts "Setting up environment..." }
setup.call
```

