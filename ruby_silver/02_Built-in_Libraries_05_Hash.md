A Hash maps each of its unique keys to a specific value.
A Hash has certain similarities to an Array, but:
- An Array index is always an Integer.
- A Hash key can be (almost) any object.

### Hash Data Syntax
```ruby
h = {:foo => 0, :bar => 1, :baz => 2}
h # => {:foo=>0, :bar=>1, :baz=>2}

h = {foo: 0, bar: 1, baz: 2}
h # => {:foo=>0, :bar=>1, :baz=>2}

h = {'foo': 0, 'bar': 1, 'baz': 2}
h # => {:foo=>0, :bar=>1, :baz=>2}

h = {foo: 0, :bar => 1, 'baz': 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

But it’s an error to try the JSON-style syntax for a key that’s not a bareword or a String:
```ruby
# Raises SyntaxError (syntax error, unexpected ':', expecting =>):
h = {0: 'zero'}
```

Hash value can be omitted, meaning that value will be fetched from the context by the name of the key:
```ruby
x = 0
y = 100
h = {x:, y:}
h # => {:x=>0, :y=>100}
```

### Creating a Hash¶
```ruby
h = Hash.new
h # => {}
h.class # => Hash

h = Hash[]
h # => {}

h = Hash[foo: 0, bar: 1, baz: 2]
h # => {:foo=>0, :bar=>1, :baz=>2}

h = {}
h # => {}

h = {foo: 0, bar: 1, baz: 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

### Hash Value Basics
```ruby
h = {foo: 0, bar: 1, baz: 2}
h[:foo] # => 0
```

```ruby
h = {foo: 0, bar: 1, baz: 2}
h[:bat] = 3 # => 3
h # => {:foo=>0, :bar=>1, :baz=>2, :bat=>3}
h[:foo] = 4 # => 4
h # => {:foo=>4, :bar=>1, :baz=>2, :bat=>3}
```

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.delete(:bar) # => 1
h # => {:foo=>0, :baz=>2}
```

### Entry Order
A Hash object presents its entries in the order of their creation

A new Hash has its initial ordering per the given entries:
```ruby

```







