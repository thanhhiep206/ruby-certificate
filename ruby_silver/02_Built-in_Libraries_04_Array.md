An Array is an ordered, integer-indexed collection of objects, called elements. Any object may be an Array element.

### Array Indexes
Array indexing starts at 0, as in C or Java.

### Creating Arrays

An Array can contain different types of objects. For example, the array below contains an Integer, a String and a Float:
```ruby
a = [1, 'hello', 3.14]
``` 
An array can also be created by calling Array.new with zero, one (the initial size of the Array) or two arguments (the initial size and a default object).
```ruby
Array.new # => []
Array.new(3) # => [nil, nil, nil]
Array.new(3, 'abc') # => ['abc', 'abc', 'abc']

(1..3).to_a # => [1, 2, 3]
(1...3).to_a # => [1, 2]
```

### Adding Items to Arrays

#### unshift 
Will add a new item to the beginning of an array.
```ruby
a = [1, 2, 3]
a.unshift(0) # => [0, 1, 2, 3]

a = [1, 2, 3]
a.unshift # => [1, 2, 3]
```

#### insert
Will add a new item to an array at a specific index.
```ruby
a = [1, 2, 3]
a.insert(1, 'hello') # => [1, 'hello', 2, 3]
```

### Removing Items from an Array
#### pop
Will remove the last item from an array and return it.
```ruby
a = [1, 2, 3]
a.pop # => 3
a # => [1, 2]
```

#### shift
Will remove the first item from an array and return it.
```ruby
a = [1, 2, 3]
a.shift # => 1
a # => [2, 3]
```

#### compact
Removes nil elements from an array; returns a new array.
```ruby
a = [1, nil, 2, nil, 3]
a.compact # => [1, 2, 3]
```

#### delete_if = reject!
Deletes every element of self for which block evaluates to true.
```ruby
scores = [ 97, 42, 75 ]
scores.delete_if {|score| score < 80 }   #=> [97]
```

### Iterating over Arrays
#### each
#### reverse_each
#### map

### Selecting Items from an Array
#### Non-destructive Selection
```ruby
arr = [1, 2, 3, 4, 5, 6]
arr.select {|a| a > 3}       #=> [4, 5, 6]
arr.reject {|a| a < 3}       #=> [3, 4, 5, 6]
arr.drop_while {|a| a < 4}   #=> [4, 5, 6]
arr                          #=> [1, 2, 3, 4, 5, 6]
```

- flatten
Returns a new array that is a one-dimensional flattening of self (recursively).
```ruby
a = [ 1, [2, [3, [4, 5]]], 6, [[[7, 8], 9]] ]
a.flatten # => [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

- flatten!
Flattens self in place.
```ruby
a = [ 1, [2, [3, [4, 5]]], 6, [[[7, 8], 9]] ]
a.flatten! # => [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

- each_cons
Iterates over the array in groups of length n, passing each group as a list to the block.
```ruby
a = [1, 2, 3, 4]
a.each_cons(3) { |cons| p cons }
# => [1, 2, 3]
# => [2, 3, 4]
```
