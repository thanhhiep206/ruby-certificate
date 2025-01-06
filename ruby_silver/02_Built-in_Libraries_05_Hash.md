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

h = Hash({})
h # => {}

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
h = Hash[foo: 0, bar: 1]
h # => {:foo=>0, :bar=>1}
```
New entries are added at the end:
```ruby
h[:baz] = 2
h # => {:foo=>0, :bar=>1, :baz=>2}
```
Updating a value does not affect the order:
```ruby
h[:baz] = 3
h # => {:foo=>0, :bar=>1, :baz=>3}
```
But re-creating a deleted entry can affect the order:
```ruby
h.delete(:foo)
h[:foo] = 5
h # => {:bar=>1, :baz=>3, :foo=>5}
```

### Hash Keys
#### Hash Key Equivalence
Two objects are treated as the same hash key when their hash value is identical and the two objects are eql? to each other.

#### Modifying an Active Hash Key
Modifying a Hash key while it is in use damages the hash’s index.

This Hash has keys that are Arrays:
```ruby
a0 = [ :foo, :bar ]
a1 = [ :baz, :bat ]
h = {a0 => 0, a1 => 1}
h.include?(a0) # => true
h[a0] # => 0
a0.hash # => 110002110
```

Modifying array element a0[0] changes its hash value:
```ruby
a0[0] = :bam
a0.hash # => 1069447059
```

And damages the Hash index:
```ruby
h.include?(a0) # => false
h[a0] # => nil
```

You can repair the hash index using method rehash:
```ruby
h.rehash # => {[:bam, :bar]=>0, [:baz, :bat]=>1}
h.include?(a0) # => true
h[a0] # => 0
```

A String key is always safe. That’s because an unfrozen String passed as a key will be replaced by a duplicated and frozen String:
```ruby
s = 'foo'
s.frozen? # => false
h = {s => 0}
first_key = h.keys.first
first_key.frozen? # => true
```

#### User-Defined Hash Keys
To be useable as a Hash key, objects must implement the methods hash and eql?. Note: this requirement does not apply if the Hash uses compare_by_identity since comparison will then rely on the keys’ object id instead of hash and eql?.

Object defines basic implementation for hash and eq? that makes each object a distinct key. Typically, user-defined classes will want to override these methods to provide meaningful behavior, or for example inherit Struct that has useful definitions for these.

A typical implementation of hash is based on the object’s data while eql? is usually aliased to the overridden == method:
```ruby
class Book
  attr_reader :author, :title

  def initialize(author, title)
    @author = author
    @title = title
  end

  def ==(other)
    self.class === other &&
      other.author == @author &&
      other.title == @title
  end

  alias eql? ==

  def hash
    @author.hash ^ @title.hash # XOR
  end
end

book1 = Book.new 'matz', 'Ruby in a Nutshell'
book2 = Book.new 'matz', 'Ruby in a Nutshell'

reviews = {}

reviews[book1] = 'Great reference!'
reviews[book2] = 'Nice and compact!'

reviews.length #=> 1
```

#### Default Values
The methods [], values_at and dig need to return the value associated to a certain key. When that key is not found, that value will be determined by its default proc (if any) or else its default (initially ‘nil`).

You can retrieve the default value with method default:
```ruby
h = Hash.new
h.default # => nil
```

You can set the default value by passing an argument to method Hash.new or with method default=
```ruby
h = Hash.new(-1)
h.default # => -1
h.default = 0
h.default # => 0
```

This default value is returned for [], values_at and dig when a key is not found:
```ruby
counts = {foo: 42}
counts.default # => nil (default)
counts[:foo] = 42
counts[:bar] # => nil
counts.default = 0
counts[:bar] # => 0
counts.values_at(:foo, :bar, :baz) # => [42, 0, 0]
counts.dig(:bar) # => 0
```

Note that the default value is used without being duplicated. It is not advised to set the default value to a mutable object:
```ruby
synonyms = Hash.new([])
synonyms[:hello] # => []
synonyms[:hello] << :hi # => [:hi], but this mutates the default!
synonyms.default # => [:hi]
synonyms[:world] << :universe
synonyms[:world] # => [:hi, :universe], oops
synonyms.keys # => [], oops
```

#### Default Proc
When the default proc for a Hash is set (i.e., not nil), the default value returned by method [] is determined by the default proc alone.

You can retrieve the default proc with method default_proc:
```ruby
h = Hash.new
h.default_proc # => nil
```

You can set the default proc by calling Hash.new with a block or calling the method default_proc=
```ruby
h = Hash.new { |hash, key| "Default value for #{key}" }
h.default_proc.class # => Proc
h.default_proc = proc { |hash, key| "Default value for #{key.inspect}" }
h.default_proc.class # => Proc
```

### Methods
#### destruction
- clear
- update
Returns a new hash created by merging the original hash with the argument hash.
```ruby
h = { "n" => 100, "m" => 100, "y" => 300, "d" => 200 }
h.update({"a" => 0 }) # => {"n"=>100, "m"=>100, "y"=>300, "d"=>200, "a"=>0}
```

#### Non-destructive
- merge
- invert
Return a new hash created by using the values from the original hash as keys, and the keys as values.
```ruby
h = { "n" => 100, "m" => 100, "y" => 300, "d" => 200, "a" => 0 }
h.invert   #=> {0=>"a", 100=>"m", 200=>"d", 300=>"y"}
```

- member
Returns true if the given key is present in the hash.
```ruby
h = { "n" => 100, "m" => 100, "y" => 300, "d" => 200, "a" => 0 }
h.member?("a") # => true
```
