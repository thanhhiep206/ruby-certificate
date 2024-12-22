### Note
- If the test expression evaluates to the constant false or nil, the test is false; otherwise, it is true.
- The number zero is considered true

### if expression
### unless expression
### if-elsif-else expression
### short-if expression (aka ternary operator)
### case expression
- An alternative to the if-elsif-else expression (above) is the case expression. Case in Ruby supports a number of syntaxes. For example, suppose we want to determine the relationship of a number (given by the variable a) to 5. We could say:
```ruby
 a = 1
 case 
 when a < 5 then puts "#{a} is less than 5"    
 when a == 5 then puts "#{a} equals 5"   
 when a > 5 then puts "#{a} is greater than 5" 
 end
```

### Loops
#### while
#### until
The until statement is similar to the while statement in functionality. Unlike the while statement, the code block for the until loop will execute as long as the expression evaluates to false.

### Keywords
#### return
