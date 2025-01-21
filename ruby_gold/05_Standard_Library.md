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
