### Syntax
- Blocks can be defined enclosing code in do and end, or curly braces {}.
- Use curly braces {} for blocks, when the code fits on one line.
```ruby
[1, 2, 3].each { |num| puts num }

[1, 2, 3].each do |num|
  puts num
end
```

- Difference between blocks and methods is that blocks are not objects.
- No need to name the block explicitly

### Yield Keyword
- Yield keyword is used to pass the block to the method
```ruby
def test
  yield
end

test { puts "Hello, World!" }
```

### Closures in Ruby
- A closure is a function that can access variables from its parent scope even after the parent function has returned.
- In Ruby, blocks are closures.
- A block can access variables from its parent scope even after the parent function has returned.
```ruby
def test
  x = 1
  -> { x += 1 }
end

closure = test
puts closure.call
puts closure.call
```

### Custom Enumerable Methods using Blocks
- Custom enumerable methods can be created using blocks.
- e.g., `each`, `map`, `select`, `reject`, `inject`, `reduce`

### Block Scoping and Binding
- Variables inside a block can shadow variables outside the block.
```ruby
x = 10
3.times do |x|
  puts x # x inside block shadows outer x
end
puts x # => 10
```

### Performance Considerations
- Block performance compares to traditional looping constructs 
- Dive into lazy enumerators and when to use them for performance benefits:
```ruby
[1, 2, 3].lazy.map { |x| x * 2 }.first
```

### Block Arguments
- Adding & to the argument name makes it a block argument.
- Block arguments are written after other arguments.
```ruby
def test(&block)
  block.call
end

test { puts "Hello, World!" }
```

```ruby
def test(n, &block)
  block.call
end

test(5) { puts "Hello, World!" } # => Hello, World!
```

```ruby
def test(n, &block)
  block.yield
end

test(5) { puts "Hello, World!" } # => Hello, World!
```

```ruby
def test(&block, n)
  block.call
end 
# => syntax error
```
