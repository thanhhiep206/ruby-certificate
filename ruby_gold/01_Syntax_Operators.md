### Ruby Operator Precedence
List of Ruby operators in order of precedence (from highest to lowest):
1. Parentheses
- ()
- Used to explicitly group expressions or change precedence.

2. Method calls
- Without parentheses.
- Example: obj.method arg.

3. Unary Operators
- !, ~, +, - (logical NOT, bitwise NOT, unary plus, unary minus).
- Example: -a, !b, +c, ~d.

4. Exponentiation
- **
- Example: 2 ** 3.

5. Multiplicative
- *, /, % (multiplication, division, modulo).
- Example: a * b / c.

6. Additive
- +, - (addition, subtraction).
- Example: a + b - c.

7. Bitwise Shifts
- <<, >> (left shift, right shift).
- Example: a << b.

8. Bitwise AND
- &
- Example: a & b.

9. Bitwise OR, XOR
- |, ^ (bitwise OR, bitwise XOR).
- Example: a | b.

10. Comparison Operators
- <, <=, >, >=, <=> (less than, greater than, etc.).
- Example: a < b.

11. Equality Operators
- ==, ===, !=, =~, !~ (equality, pattern matching).
- Example: a == b.

12. Logical AND
- &&
- Example: a && b.

13. Logical OR
- ||
- Example: a || b.

14. Range Operators
- .., ... (inclusive and exclusive ranges).
- Example: 1..5.

15. Ternary Conditional
- ? : (ternary operator).
- Example: condition ? value1 : value2.

16. Assignment
- =, +=, -=, *=, /=, %=, **=, <<=, >>=, &=, |=, ^= (and other compound assignments).
- Example: a += b.

17. Defined? Operator
- defined?.
- Example: defined? a.

18. Logical not
- not.
- Lower precedence than && and ||.
- Example: not a.

19. Logical or, and
- or, and.
- Lower precedence than || and &&.
- Example: a or b.

20. Modifiers
- if, unless, while, until.
- Example: puts a if b.

21. Statement Modifiers
- begin...end blocks with rescue, ensure.
- Example
```ruby
begin
  # code
rescue
  # code
end
```

### Note:
- Remember that and and or have lower precedence than && and ||. Use &&/|| for logical operations in most cases to avoid unexpected behavior.
```ruby
a = true and false  # Assigns `true` to `a`, then evaluates `and false`.
a = true && false   # Assigns `false` to `a`.
```

- Block is passed to the method as a parameter.
```ruby
def m1(*)
  str = yield if block_given?
  p "m1 #{str}"
end

def m2(*)
  str = yield if block_given?
  p "m2 #{str}"
end

m1 m2 do
  "hello"
end

# => 
# m1(m2) do
#   "hello"
# end

# => 
# "m2 "
# "m1 hello"
```

```ruby
m1 m2 {
  "hello"
}

# => 
# "m2 hello"
# "m1"
```

