Numeric is the class from which all higher-level numeric classes should inherit.

Numeric allows instantiation of heap-allocated objects.
Other core numeric classes such as Integer are implemented as immediates, which means that each Integer is a single immutable object which is always passed by value.

```ruby
a = 1
1.object_id == a.object_id   #=> true
```

Class Numeric:
- Inherits from class Object.
- Includes module Comparable.

### Querying

#### finite?
Returns true unless self is infinite or not a number.

#### infinite?
Returns -1, nil or +1, depending on whether self is -Infinity<tt>, finite, or <tt>+Infinity.

#### integer?

#### negative?

#### nonzero?

#### positive?

#### real?

#### zero?


### Comparing
#### <=>
- -1 if self is less than the given value.
- 0 if self is equal to the given value.
- 1 if self is greater than the given value.
- nil if self and the given value are not comparable.

#### eql?
Returns whether self and the given value have the same value and type.

### Converting
#### % (aliased as modulo)
Returns the remainder of self divided by the given value.

#### -@
Returns the value of self, negated.

#### abs (aliased as magnitude) 
Returns the absolute value of self.

#### abs2
Returns the square of self.

#### ceil
Returns the smallest number greater than or equal to self, to a given precision.

#### div
Returns the value of self divided by the given value and converted to an integer.

#### floor
Returns the largest number less than or equal to self, to a given precision.

#### round
Returns the value of self rounded to the nearest value for the given a precision.

#### to_int
Returns the Integer representation of self, truncating if necessary.

#### truncate
Returns self truncated (toward zero) to a given precision.

### Other
#### clone
Returns self; does not allow freezing.

#### dup (aliased as +@)
Returns self.

#### step
Invokes the given block with the sequence of specified numbers.



