### Basics of Numbered Parameters
- Learn that _1, _2, _3, etc., are implicit block parameters that correspond to the first, second, third arguments passed to a block.
```ruby
[1, 2, 3].map { _1 * 2 } # => [2, 4, 6]
```

### Use Cases for Numbered Parameters
- Short and Simple Blocks: Use them for concise and straightforward logic.
```ruby
(1..5).map { _1**2 } # => [1, 4, 9, 16, 25]
```

- Methods with Multiple Parameters: They simplify cases where you deal with multiple arguments.
```ruby
[[1, 2], [3, 4]].map { _1 + _2 } # => [3, 7]
```

- Mixed Parameters: Mixing numbered parameters (_1) with explicitly named ones causes a SyntaxError.
```ruby
[1, 2, 3].each { |x| puts _1 } # SyntaxError
```
