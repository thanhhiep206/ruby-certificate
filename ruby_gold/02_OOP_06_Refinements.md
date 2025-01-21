- Refinements are a way to modify the behavior of a class or module without changing the original source code.

- You cannot activate refinements dynamically (e.g., inside a method or block).
- Refinements do not work in eval, instance_eval, or similar methods when invoked in a different scope.
- Refinements are not applied to methods defined outside of the refined class/module.
```ruby
module ArrayExtensions
  refine Array do
    def summ
      inject(0, :+)
    end
  end
end

using ArrayExtensions

puts [1, 2, 3].summ # Outputs: 6
```

- Refinements vs Monkey Patching

| Aspect      | Refinements                                           | Monkey Patching                                           |
|-------------|-------------------------------------------------------|-----------------------------------------------------------|
| Scope       | Limited to specific contexts where using is declared  | Affects the entire application globally                   |
| Safety      | Safer, as changes do not affect unrelated code	      | Risky, as it can unintentionally alter unrelated behavior |
| Reusability | Explicitly enabled and reusable	                      | Always active once defined                                |
| Visibility  | Changes are isolated	                              | Changes are global                                        |
