# Ruby Association Certified Ruby Examination Gold Questions (16/07/2025)
**Note**: I only remember the question format, so I won't provide the answer.

**Q1.
```ruby
Class.superclass
```

**Q2.
```ruby
CustomError = Class.new(StandardError)

begin
  raise CustomError
rescue => e
  puts "1"
rescue CustomError => e
  puts "2"
end
```

**Q3.
```ruby
class Stack
  def initialize
    @contents = []
  end

  [:push, :pop].each do |name|
  define_method(name) do |*args|
    @contents.send(name, *args)
  end
end

stack = Stack.new
stack.push("foo")
stack.push("bar")
p stack.pop
p stack[0]
```

**Q4.
```ruby
class A
  def foo
    self.bar
  end

  private

  def bar
    "baz"
  end

  def self.bar
    "quux"
  end
end

puts A.new.foo
```

**Q5.
```ruby
a = (1/2r)
puts 2 * a
```

**Q6.
```ruby
class Identity
  def self.this_object
    self.class
  end
  
  def this_object
    self
  end
end

p Identity.this_object.class
p Identity.new.this_object.class
```

**Q7.
```ruby
require "date"

date = Date.new(2000, 2, 24)

puts(date __1__ 3)

[result]
27/02/2000
```

**Q8.
```ruby
p (1..).lazy.map { |e| e**2 }.take(3).force.sum
```

**Q9.
```ruby
%r|(http://www(\.)(.*)/)| =~ "http://www.abc.com/"

# => $1, $2, $3
```

**Q8.
```ruby
class Sample
  def hoge
    self.fuga
  end

  private

  def fuga
    puts "fuga"
  end
end
Sample.new.hoge
```

**Q9.
```ruby
h = [1, 2, 3]
case h
__(1)__ [x, y]
  p [:two, x, y]
__(1)__  [x, y, z]
  p [:three, x, y, z]
end

# [Execution Result]
# [:three, 1, 2, 3]
```

**Q10.
```ruby
class C
  def hoge
    puts "A"
  end
end

module M
  __(1)__ C do
    def hoge
      puts "B"
    end
  end
end

c = C.new
c.hoge
__(2)__ M
c.hoge
```

**Q11.
```ruby
 
require 'date'
d = Date.today - Date.new(2015,10,1)
p d.class
```

**Q12.
```ruby
class S
  @@val = 0
  def initialize
    @@val += 1
  end
end

class C < S
  class << C
    @@val += 1
  end
end

C.new
C.new
S.new
S.new

p C.class_variable_get(:@@val)
p S.class_variable_get(:@@val)
```

**Q13.
```ruby
hashes = [{name: "Meagan" }, { name: "Meagan" }, { name: "Lauren" }]
hashes.tally
hashes.chunk { |h| h[:name] }.each_with_object({}) { |(name, group), hash| hash[{:name => name}] = group.size }
```

**Q14.
```ruby
def fx(*args)
  p(args)
end
fx(["apple", "banana", "carrot"])
```

**Q15.
```ruby
def med_2(a, *b, **c, &block)
 puts a
 pust b
 pust c
 puts &block
end

def med_1(__1__)
 med_2(__2__)
end

med_1(1, 2, 3, x: 3)

# [result]
# [1]
# [2, 3]
# {x: 3}
#nil
```

```
A/ 
__1__: ...
__2__: (a, *b,  **c, &d)

B/ 
__1__: ...
__2__: ..

C/ 
__1__: (a, *b,  **c, &d)
__2__: ...

D/
__1__: (a, *b,  **c, &d)
__2__: (a, *b,  **c, &d)
```

**Q16.
```ruby
module I
end

module P
end

class C
  include I
  prepend P
end

p C.ancestors
```

**Q17.
```ruby
class Foo
  def __(1)__
    p "#{a}"
  end
end
__(2)__
```

**Q18.
```ruby
def reader_method(s)
  <<__1__
    foo
    bar
  EOF
end

# ~EOF
# -EOF.gsub...
```

**Q18.
```ruby
class BaseClass
  private

  def greet
    puts "Hello World!"
  end
end

class ChildClass < BaseClass
  def greet
   super
  end
end


ChildClass.new.greet
```

**Q19.
```ruby
__FILE__ == $0
```

**Q20.
What result is similar to the following code?
```ruby
def foo
  p 'Hello, world!'
end

def bar
  foo
end
```
=> Options Module and Class

**Q21.
```ruby
def a
  begin
    "hello"
  ensure
    puts "Ensure called!"

    "hi"
  end
end

a # => ?
```

**Q22.
```sh
The argument can be set with default value
=> The argument can not be set with default value


The argument begins with *
=> 
The argument can not begins with *
```

