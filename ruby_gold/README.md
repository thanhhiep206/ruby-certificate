# Reference Documents for Ruby Gold

## Complete Topics
- https://www.ruby.or.jp/en/certification/examination/

## How to Study
### Install docker 3.1.x
```sh
docker run -it --rm ruby:3.1 irb
```

## Practice Resources

### Official Practice
- https://github.com/ruby-association/prep-test/blob/version3/gold.md
- https://pragprog.com/titles/ppmetr2/metaprogramming-ruby-2
- https://docs.ruby-lang.org/ja/2.0.0/doc/spec=2foperator.html#range

### Additional Practice
- https://rex.libertyfish.co.jp/
- https://learn.viblo.asia/courses/ruby-association-certified-ruby-programmer-gold-ruby-gold-4openRe7Az
- https://gist.github.com/m-haramoto/121dc43453661816f4eef8fe15f86827#string-1-20%E5%95%8F
- https://gist.github.com/sean2121/0969a089fc5cdf02dfba7b14ba331c64

### References
- https://github.com/dazhizhang/ruby-rails/blob/master/Metaprogramming%20Ruby%2C%202nd%20Edition.pdf (Outdated)

## Exam Information

**Location**: IIG 75 Giang Van Minh, Ba Dinh  
**Registration**: https://www.prometric.com/test-takers/search/ry

## Study Guide Structure

### 01. Execution Environment
📁 [`01_Execution_Environment.md`](./01_Execution_Environment.md)
- Pre-defined variables and constants  
- Ruby flags and options
- Command-line interface

### 02. Advanced Syntax Features  
📁 [`02_Advanced_Syntax.md`](./02_Advanced_Syntax.md)
- Operators and precedence
- Blocks, Procs, and Lambdas
- Pattern matching (Ruby 3.0+)
- Non-local exits (break, next, return, throw/catch)
- Numbered parameters
- Here documents

### 03. Object-Oriented Programming
📁 [`03_OOP_Methods_Arguments.md`](./03_OOP_Methods_Arguments.md)
- Method definition and calling
- Argument handling (positional, keyword, splat)
- Method return values and chaining
- Method introspection and aliasing

### 04. Access Control and Classes
📁 [`04_OOP_Access_Classes.md`](./04_OOP_Access_Classes.md)
- Access control (public, private, protected)
- Class definition and features
- Instance variables and class variables
- Singleton methods and classes
- Object introspection

### 05. Inheritance and Modules
📁 [`05_OOP_Inheritance_Modules.md`](./05_OOP_Inheritance_Modules.md)
- Class inheritance and method lookup
- Method overriding and super
- Modules and mixins (include, extend, prepend)
- Module hooks and best practices
- Important standard modules (Enumerable, Comparable)

### 06. Metaprogramming
📁 [`06_Metaprogramming.md`](./06_Metaprogramming.md)
- Dynamic method invocation (send, method_missing)
- Dynamic method definition (define_method)
- Code evaluation (eval, class_eval, instance_eval)
- Introspection and reflection
- Class and module manipulation
- Best practices and security considerations

### 07. Built-in Libraries
📁 [`07_Built_in_Libraries.md`](./07_Built_in_Libraries.md)
- Regular expressions and pattern matching
- Enumerator and lazy evaluation
- Core classes and modules

### 08. Standard Library
📁 [`08_Standard_Library.md`](./08_Standard_Library.md)
- Time and Date manipulation
- JSON and YAML processing
- Singleton and Forwardable modules
- File operations and I/O
- Threading basics
