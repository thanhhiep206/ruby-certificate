### Basics of Non-Local Exits
Non-local exits occur when control flow is abruptly transferred from one point in the code to another, bypassing intermediate code
- return

Used to exit a method and return a value.
Can cause surprising behavior when used in blocks or lambdas.
```ruby
def test
  [1, 2, 3].each do |i|
    return "Exited early!" if i == 2
  end
  "Completed iteration."
end

puts test # Outputs: "Exited early!"
```
- break

break exits a loop immediately.
```ruby
for i in 1..5
  break if i == 3
  puts i
end
```
- next

next skips the rest of the current iteration and continues with the next one.
```ruby
for i in 1..5
  next if i == 3
  puts i
end
```
- redo
- retry
- throw and catch

Provides a way to jump to a predefined point in the program, similar to exceptions but without raising an error.
```ruby
catch(:exit_point) do
  [1, 2, 3].each do |i|
    throw :exit_point, "Exited early!" if i == 2
  end
end
```
