### Ruby has two types of comments: inline and block.
- Inline comments start with the # character and continue until the end of the line
- Block comments start with =begin and end with =end. Each should start on a separate line.

#### Example
```ruby
# This is an inline comment
=begin
This is a block comment
=end
```

#### Note
- =begin and =end can not be indented

### Magic Comment
- Top-level magic comments must appear in the first comment section of a file.

#### Alternative syntax
- Magic comments may consist of a single directive (as in the example above). Alternatively, multiple directives may appear on the same line if separated by “;” and wrapped between “-*-”
```ruby
# emacs-compatible; -*- coding: big5; mode: ruby; frozen_string_literal: true -*-

p 'hello'.frozen? # => true
p 'hello'.encoding # => #<Encoding:Big5>
```

#### encoding Directive
Indicates which string encoding should be used for string literals, regexp literals and __ENCODING__:
```ruby
# encoding: big5

''.encoding # => #<Encoding:Big5>
```
Default encoding is UTF-8.
Top-level magic comments must start on the first line, or on the second line if the first line looks like #! shebang line.

#### frozen_string_literal Directive
Indicates that string literals should be allocated once at parse time and frozen.
```ruby
# frozen_string_literal: true

p 'hello'.frozen? # => true
```
Starting in Ruby 3.0, string literals that are dynamic are not frozen nor reused:
```ruby
# frozen_string_literal: true

p "Addition: #{2 + 2}".frozen? # => false
```

#### warn_indent Directive
This directive can turn on detection of bad indentation for statements that follow it:
```ruby
def foo
  end # => no warning

# warn_indent: true
def bar
  end # => warning: mismatched indentations at 'end' with 'def' at 6
``` 

#### shareable_constant_value Directive
This special directive helps to create constants that hold only immutable objects, or Ractor-shareable constants.

- Mode none: No special treatment in this mode (as in Ruby 2.x): no automatic freezing and no checks.
- Mode literal: Constants assigned to literals will be deeply-frozen
- Mode experimental_everything: All values assigned to constants are made shareable.
- Mode experimental_copy: Copy deeply and make it shareable.


### Note
- Magic comments affect only the file in which they appear; other files are unaffected
