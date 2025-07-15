# Standard Library

## Time and Date

### Time Class
```ruby
# Current time
Time.now  # => 2023-12-01 10:30:45 +0000

# Creating specific times
Time.new(2023, 12, 1, 10, 30, 45)  # => 2023-12-01 10:30:45 +0000
Time.parse("2023-12-01 10:30:45")  # Requires 'time' library

# Time calculations
time = Time.now
time + 3600    # Add 1 hour (3600 seconds)
time - 86400   # Subtract 1 day (86400 seconds)

# Formatting
Time.now.strftime("%Y-%m-%d %H:%M:%S")  # => "2023-12-01 10:30:45"
```

### Date Class
```ruby
require 'date'

# Current date
Date.today  # => #<Date: 2023-12-01>

# Creating specific dates
Date.new(2023, 12, 1)  # => #<Date: 2023-12-01>
Date.parse("2023-12-01")  # => #<Date: 2023-12-01>

# Date calculations
date = Date.today
date + 7     # Add 7 days
date - 30    # Subtract 30 days
date.next    # Next day
date.prev    # Previous day

# Date information
date.year    # => 2023
date.month   # => 12
date.day     # => 1
date.wday    # => 5 (day of week, 0-6)
```

- << and >>
The << operator generates a new Date object that is n months earlier than the current date.
The >> operator generates a new Date object that is n months later than the current date.
```ruby
puts(Date.new(2016,2,29) << 12) #=> 2015-12-28
puts(Date.new(2016,12,31) << 1) #=> 2016-11-30
puts(Date.new(2016,12,31) << 2) #=> 2016-10-31
```

### DateTime Class
```ruby
require 'date'

# Current datetime
DateTime.now  # => #<DateTime: 2023-12-01T10:30:45+00:00>

# Creating specific datetimes
DateTime.new(2023, 12, 1, 10, 30, 45)
DateTime.parse("2023-12-01T10:30:45")

# Combining Date and Time features
dt = DateTime.now
dt.to_time  # Convert to Time object
dt.to_date  # Convert to Date object

# DateTime calculation
d = DateTime.now - DateTime.new(2022,11,18)
p d.class
# => Rational
```

## JSON and YAML

### JSON Processing
```ruby
require 'json'

# Parsing JSON
json_string = '{"name": "Alice", "age": 30}'
data = JSON.parse(json_string)  # => {"name"=>"Alice", "age"=>30}

# Load JSON
JSON.load json_string # => {"name"=>"Alice", "age"=>30}

# Generating JSON
hash = { name: "Alice", age: 30 }
json_string = JSON.generate(hash)  # => '{"name":"Alice","age":30}'
# or
json_string = hash.to_json  # Same result

# Pretty printing
puts JSON.pretty_generate(hash)
# {
#   "name": "Alice",
#   "age": 30
# }
```

### YAML Processing
```ruby
require 'yaml'

# Parsing YAML
yaml_string = <<~YAML
  name: Alice
  age: 30
  hobbies:
    - reading
    - coding
YAML

data = YAML.load(yaml_string)
# => {"name"=>"Alice", "age"=>30, "hobbies"=>["reading", "coding"]}

# Generating YAML
hash = { name: "Alice", age: 30, hobbies: ["reading", "coding"] }
yaml_string = YAML.dump(hash)

# Loading from file
data = YAML.load_file("config.yml")

# Saving to file
File.write("output.yml", YAML.dump(hash))
```

## Singleton Pattern

### Using Singleton Module
```ruby
require 'singleton'

class DatabaseConnection
  include Singleton
  
  def initialize
    # Connection setup
    @connected = true
  end
  
  def query(sql)
    "Executing: #{sql}"
  end
end

# Usage
db1 = DatabaseConnection.instance
db2 = DatabaseConnection.instance
db1 == db2  # => true (same object)

# Cannot create new instances
# DatabaseConnection.new  # => NoMethodError
```

## Forwardable Module

### Method Delegation
```ruby
require 'forwardable'

class UserProfile
  extend Forwardable
  
  def initialize(user)
    @user = user
  end
  
  # Delegate methods to @user
  def_delegators :@user, :name, :email, :age
  def_delegator :@user, :full_name, :display_name
end

class User
  attr_accessor :name, :email, :age
  
  def initialize(name, email, age)
    @name, @email, @age = name, email, age
  end
  
  def full_name
    "#{@name} (#{@email})"
  end
end

user = User.new("Alice", "alice@example.com", 30)
profile = UserProfile.new(user)

profile.name          # => "Alice"
profile.email         # => "alice@example.com"
profile.display_name  # => "Alice (alice@example.com)"
```

## File Operations and I/O

### File Reading
```ruby
# Read entire file
content = File.read("example.txt")

# Read with encoding
content = File.read("example.txt", encoding: "UTF-8")

# Read lines
lines = File.readlines("example.txt")

# Block form (auto-closes file)
File.open("example.txt", "r") do |file|
  file.each_line do |line|
    puts line
  end
end
```

### File Writing
```ruby
# Write string to file
File.write("output.txt", "Hello, World!")

# Append to file
File.write("output.txt", "New line\n", mode: "a")

# Block form
File.open("output.txt", "w") do |file|
  file.puts "Line 1"
  file.puts "Line 2"
end
```

### Directory Operations
```ruby
require 'fileutils'

# Create directory
Dir.mkdir("new_directory")
FileUtils.mkdir_p("path/to/nested/directory")

# List directory contents
Dir.entries(".")           # => [".", "..", "file1.txt", ...]
Dir.glob("*.rb")          # => ["script1.rb", "script2.rb"]
Dir["**/*.rb"]            # Recursive glob

# Change directory
Dir.chdir("/tmp") do
  # Work in /tmp
  puts Dir.pwd
end

# File operations
FileUtils.copy("source.txt", "destination.txt")
FileUtils.move("old_name.txt", "new_name.txt")
FileUtils.remove("unwanted.txt")
```

### File Information
```ruby
# File existence and type
File.exist?("example.txt")     # => true/false
File.file?("example.txt")      # => true if regular file
File.directory?("example")     # => true if directory

# File stats
stat = File.stat("example.txt")
stat.size                      # File size in bytes
stat.mtime                     # Modification time
stat.ctime                     # Change time
stat.atime                     # Access time

# File permissions
File.readable?("example.txt")  # => true/false
File.writable?("example.txt")  # => true/false
File.executable?("example.txt") # => true/false
```

## Additional Standard Libraries

### Set Operations
```ruby
require 'set'

set1 = Set.new([1, 2, 3])
set2 = Set.new([3, 4, 5])

set1 | set2    # Union: #<Set: {1, 2, 3, 4, 5}>
set1 & set2    # Intersection: #<Set: {3}>
set1 - set2    # Difference: #<Set: {1, 2}>

set1.include?(2)  # => true
set1.add(6)       # Add element
set1.delete(1)    # Remove element
```

### URI Processing
```ruby
require 'uri'

uri = URI.parse("https://example.com:8080/path?query=value#fragment")
uri.scheme    # => "https"
uri.host      # => "example.com"
uri.port      # => 8080
uri.path      # => "/path"
uri.query     # => "query=value"
uri.fragment  # => "fragment"

# Building URIs
uri = URI::HTTPS.build(
  host: "api.example.com",
  path: "/v1/users",
  query: "limit=10&offset=0"
)
```

### CSV Processing
```ruby
require 'csv'

# Reading CSV
CSV.foreach("data.csv", headers: true) do |row|
  puts "Name: #{row['name']}, Age: #{row['age']}"
end

# Writing CSV
CSV.open("output.csv", "w") do |csv|
  csv << ["Name", "Age", "City"]
  csv << ["Alice", 30, "New York"]
  csv << ["Bob", 25, "San Francisco"]
end

# Parsing CSV string
data = CSV.parse("name,age\nAlice,30\nBob,25", headers: true)
data.each { |row| puts row.to_h }
```

### Fiber
- Fiber means creating code blocks that can be paused and resumed, much like threads
- When a fiber is created it will not run automatically. It must be explicitly asked to run using the `Fiber#resume` method
- The code running inside the fiber can give up control by calling `Fiber.yield` in which case it yields control back to caller (the caller of the `Fiber#resume`).
- Upon yielding or termination the Fiber returns the value of the last executed expression
```ruby
fiber = Fiber.new do
  Fiber.yield 1
  2
end

puts fiber.resume # => 1
puts fiber.resume # => 2
puts fiber.resume # => FiberError: can't resume dead fiber
```

- The `Fiber#resume` method accepts an arbitrary number of parameters, if it is the first call to resume then they will be passed as block arguments
  - Otherwise they will be the return value of the call to `Fiber.yield`
```ruby
fiber = Fiber.new do |first|
  second = Fiber.yield first + 2
end

puts fiber.resume 10 # => 12
puts fiber.resume 1_000_000 # => 1000000
puts fiber.resume "The fiber will be dead before I can cause trouble" # => FiberError: can't resume dead fiber
```

### StringIO
```ruby
require "stringio"
strio = StringIO.new # => #<StringIO>
strio.close
```

```ruby
strio = StringIO.open # => #<StringIO>
```

### Rational
- BigDecimal extends the native Rational class to provide the to_d method.

```ruby
Rational(1)      #=> (1/1)
Rational(2, 3)   #=> (2/3)
Rational(4, -6)  #=> (-2/3)
3.to_r           #=> (3/1)
2/3r             #=> (2/3)
```

```ruby
Rational(0.3)    #=> (5404319552844595/18014398509481984)
Rational('0.3')  #=> (3/10)
Rational('2/3')  #=> (2/3)

0.3.to_r         #=> (5404319552844595/18014398509481984)
'0.3'.to_r       #=> (3/10)
'2/3'.to_r       #=> (2/3)
0.3.rationalize  #=> (3/10)
```

### open-uri
The OpenURI standard library extends the functionality of Kernel#open to allow treating HTTP-based resources as if they were a file. It is a wrapper around the low-level Net::HTTP standard library.
```ruby
require "open-uri"

puts open("http://example.com").read
```
