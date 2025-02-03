- Refinements are a way to modify the behavior of a class or module without changing the original source code.

- You cannot activate refinements dynamically (e.g., inside a method or block).
- Refinements do not work in eval, instance_eval, or similar methods when invoked in a different scope.
- Refinements are not applied to methods defined outside of the refined class/module.
```ruby
class C
  def foo
    puts "C#foo"
  end
end

module M
  refine C do
    def foo
      puts "C#foo in M"
    end
  end
end

using M

c = C.new

c.foo # prints "C#foo in M"
```

- Refinements vs Monkey Patching

| Aspect      | Refinements                                           | Monkey Patching                                           |
|-------------|-------------------------------------------------------|-----------------------------------------------------------|
| Scope       | Limited to specific contexts where using is declared  | Affects the entire application globally                   |
| Safety      | Safer, as changes do not affect unrelated code	      | Risky, as it can unintentionally alter unrelated behavior |
| Reusability | Explicitly enabled and reusable	                      | Always active once defined                                |
| Visibility  | Changes are isolated	                                | Changes are global                                        |

#### Method Lookup
When looking up a method for an instance of class C Ruby checks:
- If refinements are active for C, in the reverse order they were activated:
  - The prepended modules from the refinement for C
  - The refinement for C
  - The included modules from the refinement for C
- The prepended modules of C
- C
- The included modules of C
If no method was found at any point this repeats with the superclass of C.
Note that methods in a subclass have priority over refinements in a superclass. For example, if the method / is defined in a refinement for Integer 1 / 2 invokes the original Fixnum#/ because Fixnum is a subclass of Integer and is searched before the refinements for the superclass Integer.
If a method foo is defined on Integer in a refinement, 1.foo invokes that method since foo does not exist on Fixnum.

```ruby
# not activated here
using M
# activated here
class Foo
  # activated here
  def foo
    # activated here
  end
  # activated here
end
# activated here
```

```ruby
# not activated here
class Foo
  # not activated here
  def foo
    # not activated here
  end
  using M
  # activated here
  def bar
    # activated here
  end
  # activated here
end
# not activated here
```

```ruby
# not activated here
eval <<EOF
  # not activated here
  using M
  # activated here
EOF
# not activated here
```

```ruby
# not activated here
if false
  using M
end
# not activated here
```

