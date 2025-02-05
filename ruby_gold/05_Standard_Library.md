### Time
- Time.now - Returns the current time.
- Time.new - Creates a new Time object.
- Formatting: strftime("%Y-%m-%d %H:%M:%S").
- Parsing: Time.parse('2025-01-21 12:34:56') (requires require 'time').

### Date
- A simpler alternative to Time for dates without time information.
- Date.today - Returns the current date
- Date.new(2025, 1, 21) - Creates a specific date.
- parse, strftime, and next_day for parsing, formatting, and date manipulation.

### Singleton
- Implements the Singleton pattern for classes that should have only one instance.
```ruby
require 'singleton'

class Configuration
  include Singleton
  attr_accessor :setting
end

config = Configuration.instance
config.setting = "dark_mode"
```

### Forwardable
- Delegates methods to another object.
```ruby
require 'forwardable'

class Library
  extend Forwardable
  def_delegators :@books, :push, :pop

  def initialize
    @books = []
  end
end
```

### Threading and Concurrency
- Multithreading in Ruby.
```ruby
Thread.new { puts "In a thread" }
```

### YAML
- This module provides a Ruby interface for data serialization in YAML format.
```ruby
require 'yaml'

yaml = YAML.dump({ name: "Alice", age: 30 })
puts yaml
# => ---
# => name: Alice
# => age: 30
```

```ruby
require 'yaml'

yaml = <<YAML
  sum: 510,
  orders:
    - 260
    - 250
YAML

object = YAML.load(yaml)
puts object
# => {:sum=>510, :orders=>[260, 250]}
```

### JSON
- This module provides a Ruby interface for data serialization in JSON format.
```ruby
require 'json'

json = JSON.dump({ name: "Alice", age: 30 })
puts json
# => {"name":"Alice","age":30}
```

#### Parsing JSON Arrays
```ruby
json = '[{"foo": 0, "bar": 1}, ["baz", 2]]'
JSON.parse(json) # => [{"foo"=>0, "bar"=>1}, ["baz", 2]]
```

#### Parsing JSON Objects
```ruby
json = '{"foo": {"bar": 1, "baz": 2}, "bat": [0, 1, 2]}'
JSON.parse(json) # => {"foo"=>{"bar"=>1, "baz"=>2}, "bat"=>[0, 1, 2]}
```

#### Parsing JSON Scalars
- String
```ruby
ruby = JSON.parse('"foo"')
ruby # => 'foo'
ruby.class # => String
```

- Number
```ruby
ruby = JSON.parse('123')
ruby # => 123
ruby.class # => Integer
```

- Boolean
```ruby
ruby = JSON.parse('true')
ruby # => true
ruby.class # => TrueClass
```

- Null
```ruby
ruby = JSON.parse('null')
ruby # => nil
ruby.class # => NilClass
```

#### Load JSON
```ruby
json = '{"name": "Alice", "age": 30}'
object = JSON.load(json)
object # => {"name"=>"Alice", "age"=>30}
```

#### Generating JSON
- JSON.dump
```ruby
json = JSON.dump({ name: "Alice", age: 30 })
puts json
# => {"name":"Alice","age":30}
```
