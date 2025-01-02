The Comparable mixin is used by classes whose objects may be ordered. The class must define the <=> operator, which compares the receiver against another object, returning a value less than 0, returning 0, or returning a value greater than 0, depending on whether the receiver is less than, equal to, or greater than the other object.

```ruby
class SizeMatters
  include Comparable
  attr :str
  def <=>(other)
    str.size <=> other.str.size
  end
  def initialize(str)
    @str = str
  end
  def inspect
    @str
  end
end

s1 = SizeMatters.new("Z")
s2 = SizeMatters.new("YY")
s3 = SizeMatters.new("XXX")
s4 = SizeMatters.new("WWWW")
s5 = SizeMatters.new("VVVVV")

s1 < s2                       #=> true
s4.between?(s1, s3)           #=> false
s4.between?(s3, s5)           #=> true
[ s3, s2, s5, s4, s1 ].sort   #=> [Z, YY, XXX, WWWW, VVVVV]
```

#### <
Returns whether self is less than the given object.

#### <=
Returns whether self is less than or equal to the given object.

#### >
Returns whether self is greater than the given object.

#### >=
Returns whether self is greater than or equal to the given object.

#### between?
Returns whether self is between the two given values.

#### clamp  
Clamps self to the range given by min and max.
```ruby
12.clamp(0, 100)         #=> 12
523.clamp(0, 100)        #=> 100
-3.123.clamp(0, 100)     #=> 0

'd'.clamp('a', 'f')      #=> 'd'
'z'.clamp('a', 'f')      #=> 'f'
```
