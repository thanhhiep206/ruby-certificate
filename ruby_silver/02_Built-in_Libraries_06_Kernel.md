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

#### Rational
Returns x/y or arg as a Rational.
```ruby
Rational(2, 3)   #=> (2/3)
Rational(5)      #=> (5/1)
Rational(0.5)    #=> (1/2)
Rational(0.3)    #=> (5404319552844595/18014398509481984)

Rational("2/3")  #=> (2/3)
Rational("0.3")  #=> (3/10)

Rational("10 cents")  #=> ArgumentError
Rational(nil)         #=> TypeError
Rational(1, nil)      #=> TypeError

Rational("10 cents", exception: false)  #=> nil
```

#### String

### Querying

### Exiting

### Exceptions

### IO
#### gets

#### open

#### p

#### pp

#### print

#### printf

#### putc

#### puts

#### readline

#### readlines

#### select

### Procs
#### lambda

#### proc

### Tracing

### Subprocesses

### Loading

### Yielding

### Random Values
#### rand

#### srand

### Other
#### other
#### loop
#### sleep
#### sprintf (aliased as format)
#### syscall


