The Enumerable mixin provides collection classes with several traversal and searching methods, and with the ability to sort

#### all?
Passes each element of the collection to the given block. The method returns true if the block never returns false or nil
```ruby
[1, 2, 3].all? { |n| n > 0 } # => true
[1, 2, 3].all? { |n| n > 1 } # => false
```

#### any?
Passes each element of the collection to the given block. The method returns true if the block ever returns a value other than false or nil. If the block is not given, Ruby adds an implicit block of { |obj| obj } that will cause any? to return true if at least one of the collection members is not false or nil.
```ruby
[1, 2, 3].any? { |n| n > 1 } # => true
[1, 2, 3].any? { |n| n > 3 } # => false
```

#### chain
Returns an enumerator object generated from this enumerator and given enumerables.
```ruby
[1, 2, 3].chain([4, 5], [6, 7]) # => [1, 2, 3, 4, 5, 6, 7]
```

#### chunk
Enumerates over the items, chunking them together based on the return value of the block.

```ruby
[3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5].chunk { |n|
  n.even?
}.each { |even, ary|
  p [even, ary]
}
#=> [false, [3, 1]]
#   [true, [4]]
#   [false, [1, 5, 9]]
#   [true, [2, 6]]
#   [false, [5, 3, 5]]
```

#### collect
Returns a new array with the results of running block once for every element in enum.

```ruby
(1..4).map { |i| i*i }      #=> [1, 4, 9, 16]
(1..4).collect { "cat"  }   #=> ["cat", "cat", "cat", "cat"]
```

#### detect
Passes each entry in enum to block. Returns the first for which block is not false. If no object matches, calls ifnone and returns its result when it is specified, or returns nil otherwise.
```ruby
(1..100).detect  #=> #<Enumerator: 1..100:detect>
(1..100).find    #=> #<Enumerator: 1..100:find>

(1..10).detect         { |i| i % 5 == 0 && i % 7 == 0 }   #=> nil
(1..10).find           { |i| i % 5 == 0 && i % 7 == 0 }   #=> nil
(1..10).detect(-> {0}) { |i| i % 5 == 0 && i % 7 == 0 }   #=> 0
(1..10).find(-> {0})   { |i| i % 5 == 0 && i % 7 == 0 }   #=> 0
(1..100).detect        { |i| i % 5 == 0 && i % 7 == 0 }   #=> 35
(1..100).find          { |i| i % 5 == 0 && i % 7 == 0 }   #=> 35
```

#### partition
Returns two arrays, the first containing the elements of enum for which the block evaluates to true, the second containing the rest.
```ruby
(1..6).partition { |i| i.even? } # => [[2, 4, 6], [1, 3, 5]]
```

