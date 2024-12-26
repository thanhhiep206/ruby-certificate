https://ruby-doc.org/core-3.1.0/doc/syntax/methods_rdoc.html

A method definition consists of the:
- def keyword
- a method name
- the body of the method
- return value
- the end keyword
When called the method will execute the body of the method

```ruby
def one_plus_one
  1 + 1
end
```
```ruby
def one_plus_one = 1 + 1
```

### Method Names
Method names may be one of the operators or must start a letter or a character with the eighth bit set.
It may contain letters, numbers, an _ (underscore or low line) or a character with the eighth bit set.
Method names may end with a ! (bang or exclamation mark), a ? (question mark), or = (equals sign).

These are method names for the various Ruby operators. Each of these operators accepts only one argument:
- +: add
- -: subtract
- *: multiply
- **: power
- /: divide
- %: modulus division, String#%
- &: AND
- ^: XOR
- ">>":right-shift
- <<:left-shift, append
- ==: equal
- !=: not equal
- ===: case equality
- =~: pattern match
- !~: does not match
- <=>: comparison aka spaceship operator


To define unary methods minus and plus, follow the operator with an @ as in +@
```ruby
class C
  def -@
    puts "you inverted this object"
  end
end

obj = C.new

-obj # prints "you inverted this object"
```

Additionally, methods for element reference and assignment may be defined: [] and []= respectively. Both can take one or more arguments, and element reference can take none.
```ruby
class C
  def [](a, b)
    puts a + b
  end

  def []=(a, b, c)
    puts a * b + c
  end
end

obj = C.new

obj[2, 3]     # prints "5"
obj[2, 3] = 4 # prints "10"
```

### Return Values

Note that for assignment methods the return value will be ignored when using the assignment syntax. Instead, the argument will be returned:
```ruby
def a=(value)
  return 1 + value
end

p(self.a = 5) # prints 5
```

The actual return value will be returned when invoking the method directly:
```ruby
p send(:a=, 5) # prints 6
```

### Scope

The syntax for adding a method to an object is as follows:
```ruby
greeting = "Hello"

def greeting.broaden
  self + ", world!"
end

greeting.broaden # returns "Hello, world!"

```

### Overriding

### Arguments

### Default Values
The default value does not need to appear first, but arguments with defaults must be grouped together. This is ok:
```ruby
def add_values(a = 1, b = 2, c)
  a + b + c
end
```

This will raise a SyntaxError:
```ruby
def add_values(a = 1, b, c = 1)
  a + b + c
end
```

Default argument values can refer to arguments that have already been evaluated as local variables, and argument values are always evaluated left to right. So this is allowed:
```ruby
def add_values(a = 1, b = a)
  a + b
end
add_values
# => 2
```

But this will raise a NameError (unless there is a method named b defined):
```ruby
def add_values(a = b, b = 1)
  a + b
end
add_values
# NameError (undefined local variable or method `b' for main:Object)
```

### Array Decomposition
You can decompose (unpack or extract values from) an Array using extra parentheses in the arguments:
```ruby
def my_method((a, b))
  p a: a, b: b
end

my_method([1, 2])
```

If the argument has extra elements in the Array they will be ignored:
```ruby
def my_method((a, b))
  p a: a, b: b
end

my_method([1, 2, 3])
```

You can use a * to collect the remaining arguments. This splits an Array into a first element and the rest:
```ruby
def my_method((a, *b))
  p a: a, b: b
end

my_method([1, 2, 3])
```

If the argument is not an Array it will be assigned to the first argument in the decomposition and the remaining arguments in the decomposition will be nil
```ruby
def my_method(a, (b, c), d)
  p a: a, b: b, c: c, d: d
end

my_method(1, 2, 3)

# => {:a=>1, :b=>2, :c=>nil, :d=>3}
```

### Array/Hash Argument
Prefixing an argument with * causes any remaining arguments to be converted to an Array:
```ruby
def gather_arguments(*arguments)
  p arguments
end

gather_arguments 1, 2, 3 # prints [1, 2, 3]
```

The array argument must appear before any keyword arguments.

It is possible to gather arguments at the beginning or in the middle:

```ruby
def gather_arguments(first_arg, *middle_arguments, last_arg)
  p middle_arguments
end

gather_arguments 1, 2, 3, 4 # prints [2, 3]
```

The array argument will capture a Hash as the last entry if keywords were provided by the caller after all positional arguments.
```ruby
def gather_arguments(*arguments)
  p arguments
end

gather_arguments 1, a: 2 # prints [1, {:a=>2}]
```

However, this only occurs if the method does not declare any keyword arguments.
```ruby
def gather_arguments_keyword(*positional, keyword: nil)
 p positional: positional, keyword: keyword
end

gather_arguments_keyword 1, 2, three: 3
#=> raises: unknown keyword: three (ArgumentError)
```

Also, note that a bare * can be used to ignore arguments:
```ruby
def ignore_arguments(*)
end
```

### Keyword Arguments
```ruby
def gather_arguments(first: nil, **rest)
  p first, rest
end

gather_arguments first: 1, second: 2, third: 3
# prints 1 then {:second=>2, :third=>3}
```

When mixing keyword arguments and positional arguments, all positional arguments must appear before any keyword arguments.

### Block Argument
The block argument is indicated by & and must come last:
```ruby
def my_method(&my_block)
  my_block.call(self)
end

def each_item(&block)
  @items.each(&block)
end

def each_item(&)
  @items.each(&)
end
```

If you are only going to call the block and will not otherwise manipulate it or send it to another method, using yield without an explicit block parameter is preferred. This method is equivalent to the first method in this section:
```ruby
def my_method
  yield self
end
```

### Argument Forwarding
```ruby
def concrete_method(*positional_args, **keyword_args, &block)
  [positional_args, keyword_args, block]
end

def forwarding_method(...)
  concrete_method(...)
end

forwarding_method(1, b: 2) { puts 3 }
#=>  [[1], {:b=>2}, #<Proc:...skip...>]
```

### Method Lookup
When you send a message, Ruby looks up the method that matches the name of the message for the receiver. Methods are stored in classes and modules so method lookup walks these, not the objects themselves.

Here is the order of method lookup for the receiver’s class or module R:

The prepended modules of R in reverse order

For a matching method in R

The included modules of R in reverse order

If R is a class with a superclass, this is repeated with R‘s superclass until a method is found.

Once a match is found method lookup stops.

If no match is found this repeats from the beginning, but looking for method_missing. The default method_missing is BasicObject#method_missing which raises a NameError when invoked.

If refinements (an experimental feature) are active, the method lookup changes. See the refinements documentation for details.

