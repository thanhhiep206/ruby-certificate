### String
| method | description | args | destructive? | can use !? | return value |
|--------|-------------|------|--------------|------------|--------------|
| size/length   | Returns the number of characters in the string | n/a | No | No | Integer |
| count  | Returns total number of characters in the string | string | No | No | Integer |
| chomp  | Removes the last character of the string, defaults to "\n" | string | No | Yes | String |
| scan   | Returns an array of all occurrences of the pattern in the string | string | No | No | Array |
| slice  | Returns a substring from the string | (start index, length) or character | No | Yes | String |

### Array
| method | description | args | destructive? | can use !? | return value |
|--------|-------------|------|--------------|------------|--------------|
| size/length   | Returns the number of elements in the array | n/a | No | No | Integer |
| count  | Returns total number of elements in the array | string | No | No | Integer |
| slice  | Returns a subset of the array | (start index, length) or character | No | Yes | Array |


### Hash
| method | description | args | destructive? | can use !? | return value |
|--------|-------------|------|--------------|------------|--------------|
| size/length   | Returns the number of elements in the hash | n/a | No | No | Integer |
| count  | Returns total number of elements in the hash | string | No | No | Integer |
| slice  | Returns a subset of the hash | (start index, length) or character | No | Yes | Hash |


#### Note
- Destructive methods modify the original object and return the modified object if it has been modified or nil if it has not been modified.
- Non-destructive methods do not modify the original object and return a new object.

```ruby
str = "Hello, world!"
puts str.upcase # => "HELLO, WORLD!"
puts str.upcase! # => "HELLO, WORLD!"
puts str.upcase! # => nil
```
