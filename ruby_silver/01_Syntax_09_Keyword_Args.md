### How Keyword Arguments Work
Ruby is unusual in that it allows you to specify method arguments as being callable either by position or by keyword but not both
In Ruby, the method definition determines whether an argument is positional or keyword and the caller has to match. With one exception that we’ll get to in a bit.
In a method definition, you declare keyword arguments by putting a colon after the argument name. Inside the method, the argument is usable as a local variable.
```ruby
def basic_method(name:, address:, country:)
  p "#{name}, #{address}, #{country}"
end
```

All the keyword arguments are required, and failing to include all of them in a method call results in an ArgumentError:
```ruby
basic_method(name: "Bruce", address: "Gotham")
# `basic_method': missing keyword: :country (ArgumentError)
```

You can actually take advantage of the way Ruby evaluates default values left to right to create a method that will take ether keyword or positional arguments:
```ruby
def dual_interface(_name = nil, _state = nil, name: _name, state: _state)
  p "#{name}: #{state}"
end

> dual_interface("Noel", "IL")
=> "Noel: IL"
> dual_interface(name: "Noel", state: "IL")
=> "Noel: IL"
```

### Keyword Arguments and Hashes

On the method definition side, you can specify an arbitrary amount of key/value arguments with a double splat, or **, prefixing the argument name:
```ruby
def a_double_splat(name:, **options)
  p options
end

a_double_splat(name: "Peter", height: 72, eyes: "Blue")
> => {:height=>72, :eyes=>"Blue"}
```


First off, the value to the right of the ** in the method call can be something other than a Hash. The object must respond to the method to_hash, Ruby will call to_hash and use the resulting hash as the ** target:
```ruby
class User
  def initialize(first_name, last_name, address)
    @first_name = first_name
    @last_name = last_name
    @address = address
  end

  def to_hash
    {name: "#{@first_name} #{@last_name}",
     address: @address, country: "USA"}
  end
end

> user = User.new("Steve", "Rogers", "Brooklyn")
> > basic_method(**user)
=> "Steve Rogers, Brooklyn, USA"
```

Can do the same thing with single splat * and to_a:
```ruby
class User
  def to_a
    [@first_name, @last_name, @address]
  end
end

def array_method(first, last, address)
  p "#{first} #{last} #{address}"
end
> array_method(*user)
=> "Steve Rogers Brooklyn"
```
