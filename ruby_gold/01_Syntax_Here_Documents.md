### Here Document
See [Here Document in Ruby Silver](../ruby_silver/01_Syntax_02_Literals.md#here-document)

can removes leading whitespace
```ruby
str = <<~TEXT
  This is a heredoc.
  Leading spaces are removed.
TEXT
puts str
```

concatenate heredocs to form complex strings
```ruby
str = <<TEXT1 + <<TEXT2
Hello from the first heredoc.
TEXT1
Hello from the second heredoc.
TEXT2
puts str
```

using for Shell command
```ruby
str = <<SHELL
echo "Hello, World!"
SHELL
puts `#{str}`
```

Embedding Heredocs in Data Structures
```ruby
data = {
  title: <<TITLE,
Welcome to Ruby!
TITLE
  body: <<BODY
This is a detailed description.
BODY
}
puts data[:title]
puts data[:body]
```

To call a method on a heredoc place it after the opening identifier
```ruby
puts(<<-ONE)
content for heredoc one
ONE

puts(<<-ONE
content for heredoc one
ONE
)

puts(<<-ONE, <<-TWO)
content for heredoc one
ONE
content for heredoc two
TWO
```
