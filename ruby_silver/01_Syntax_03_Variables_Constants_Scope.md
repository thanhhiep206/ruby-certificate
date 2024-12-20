### Variables
Type of variables in Ruby:
- Local variables:
  - Start with a lowercase letter or **underscore**
  - Scope is limited to the block, method, or class in which they are defined
- Instance variables:
  - Start with a `@` symbol
  - Scope is limited to the instance of the class
- Class variables:
  - Start with a `@@` symbol
  - Scope is limited to the class
- Global variables:
  - Start with a `$` symbol
  - Can be accessed from anywhere in the program but should be avoided

### Constants
- Start with an uppercase letter
- Use constants to store values that should not be changed (can be changed in some cases but ruby will warn you)
- Scope is limited to the class or module in which they are defined

### Scope
- Variables cannot be used outside of the scope in which they are defined
```ruby
def my_method
  my_variable = 10
end

puts my_variable #=> NameError: undefined local variable or method `my_variable'
```

- Constants lookup
Ruby finds constants in the following order:
1. Current class or module
2. Parent classes or modules
3. Global scope

- Block scope
In a block, can access variables outside of the block but cannot modify them
```ruby
x = 10
[1, 2, 3].each do |x|
  x += 1
end
puts x #=> 10
```
