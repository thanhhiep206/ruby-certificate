Exceptions are rescued in a begin/end block:
```ruby
begin
  # code that might raise
rescue
  # handle exception
end
```

If you are inside a method, you do not need to use begin or end unless you wish to limit the scope of rescued exceptions:
```ruby
def my_method
  # code that might raise
rescue
  # handle exception
end
```

The same is true for a class, module, and block:
```ruby
[0, 1, 2].map do |i|
  10 / i
rescue ZeroDivisionError
  nil
end
#=> [nil, 10, 5]
```

You can assign the exception to a local variable by using => variable_name at the end of the rescue line:
```ruby
begin
  # ...
rescue => exception
  warn exception.message
  raise # re-raise the current exception
end
```

By default, StandardError and its subclasses are rescued. You can rescue a specific set of exception classes (and their subclasses) by listing them after rescue:
```ruby
begin
  # ...
rescue ArgumentError, NameError
  # handle ArgumentError or NameError
end
```

You may rescue different types of exceptions in different ways:
```ruby
begin
  # ...
rescue ArgumentError
  # handle ArgumentError
rescue NameError
  # handle NameError
rescue
  # handle any StandardError
end
```

You may retry rescued exceptions:
```ruby
begin
  # ...
rescue
  # do something that may change the result of the begin block
  retry
end
``` 
Inside a rescue block is the only valid location for retry, all other uses will raise a SyntaxError. If you wish to retry a block iteration use redo. See Control Expressions for details.


To always run some code whether an exception was raised or not, use ensure:
```ruby
begin
  # ...
rescue
  # ...
ensure
  # this always runs
end
```

You may also run some code when an exception is not raised:
```ruby
begin
  # ...
rescue
  # ...
else
  # this runs only when no exception was raised
ensure
  # ...
end
```
