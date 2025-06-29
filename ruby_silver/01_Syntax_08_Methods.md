### Module

A Module is a collection of methods and constants. The methods in a module may be instance methods or module methods.
Instance methods appear as methods in a class when the module is included, module methods do not.

``` ruby
module Mod
  include Math
  CONST = 1
  def meth
    #  ...
  end
end
Mod.class              #=> Module
Mod.constants          #=> [:CONST, :PI, :E]
Mod.instance_methods   #=> [:meth]
```

### Public Instance Methods

#### mod < other → true, false, or nil
Returns true if mod is a subclass of other. Returns nil if there's no relationship between the two. (Think of the relationship in terms of the class definition: “class A < B” implies “A < B”.)

#### mod <= other → true, false, or nil
Returns true if mod is a subclass of other or is the same as other. Returns nil if there's no relationship between the two. (Think of the relationship in terms of the class definition: “class A < B” implies “A < B”.)

#### module <=> other_module → -1, 0, +1, or nilclick to toggle 
Comparison—Returns -1, 0, +1 or nil depending on whether module includes other_module, they are the same, or if module is included by other_module.

Returns nil if module has no relationship with other_module, if other_module is not a module, or if the two values are incomparable.

#### ==, equal?, eql?
- ==: Returns true if obj and other have the same value.
- equal?: Returns true if obj and other are the same object (memory reference: object_id)
- eql?: Returns true if obj and other have the same value and data type.

```ruby
"hello" == "hello"  # true (Giá trị giống nhau)
1 == 1.0            # true (Giá trị số học giống nhau)
[1, 2] == [1, 2]    # true (Các phần tử giống nhau)

a = "hello"
b = "hello"
a.equal?(b)         # false (Hai đối tượng khác nhau trong bộ nhớ)
c = a
a.equal?(c)         # true (Cùng tham chiếu đến cùng một đối tượng)

1.eql?(1)           # true (Giống nhau về giá trị và kiểu Integer)
1.eql?(1.0)         # false (Khác kiểu: Integer và Float)
{1 => "one"}[1.0]   # nil (Do 1 và 1.0 không eql?)
{1 => "one"}[1]     # "one" (Do 1 và 1 eql?)
```

| Method | Description | Can be overridden? |
|--------|-------------|---------|
| == | Value | Yes |
| equal? | Same object (memory reference) | No |
| eql? | Value and data type | Yes |

#### mod === obj → true or false
Returns true if obj is an instance of mod or an instance of one of mod's descendants.

#### mod > other → true, false, or nil
Returns true if mod is an ancestor of other. Returns nil if there's no relationship between the two. (Think of the relationship in terms of the class definition: “class A < B” implies “B > A”.)

#### mod >= other → true, false, or nil
Returns true if mod is an ancestor of other, or the two modules are the same. Returns nil if there's no relationship between the two. (Think of the relationship in terms of the class definition: “class A < B” implies “B > A”.)

#### alias_method(new_name, old_name) → symbolclick to to
Makes new_name a new copy of the method old_name.

#### ancestors → array
Returns a list of modules included/prepended in mod (including mod itself).

#### attr(name, ...)
- attr(name)
- attr(name, true)
- attr(name, false)
The first form is equivalent to attr_reader. The second form is equivalent to attr_accessor(name) but deprecated. The last form is equivalent to attr_reader(name) but deprecated

#### attr_accessor (getter and setter)
#### attr_reader (getter)
#### attr_writer (setter)
