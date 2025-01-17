The Kernel module is included by class Object, so its methods are available in every Ruby object.

These methods are called without a receiver and thus can be called in functional form:
```ruby
sprintf "%.1f", 1.234 #=> "1.2"
```

### Converting
#### Array
Returns arg as an Array.
```ruby
Array(1..5) #=> [1, 2, 3, 4, 5]
```

#### Complex

#### Float

#### Hash

#### Integer
Converts arg to an Integer
```ruby
Integer(123.999)    #=> 123
Integer("0x1a")     #=> 26
Integer(Time.new)   #=> 1204973019
Integer("0930", 10) #=> 930
Integer("111", 2)   #=> 7
Integer(" +1_0 ")   #=> 10
Integer(nil)        #=> TypeError: can't convert nil into Integer
Integer("x")        #=> ArgumentError: invalid value for Integer(): "x"

Integer("x", exception: false)        #=> nil
```

#### String

### Querying
