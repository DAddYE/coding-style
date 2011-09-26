## Formatting

* Use UTF-8 as the source file encoding.
* Use two-space indent, no tabs. (Your editor/IDE should have a setting to
  help you with that.)
* Use Unix-style line endings. (Linux/OSX users are covered by default,
  Windows users have to be extra careful.)
    * If you're using Git you might want to add the following
    configuration setting to protect your project from Windows line
    endings creeping in:

        ```$ git config --global core.autocrlf true```

* Use spaces around operators, after commas, colons and semicolons, around {
  and before }.

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts "Hi"
    [1, 2, 3].each { |e| puts e }
    ```

* No spaces after (, [ or before ], ).

    ```Ruby
    some(arg).other
    [1, 2, 3].length
    ```

* Indent `when` as deep as `case`. (As suggested in the Pickaxe.)

    ```Ruby
    case
    when song.name == "Misty"
      puts "Not again!"
    when song.duration > 120
      puts "Too long!"
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end

    kind = case year
           when 1850..1889 then "Blues"
           when 1890..1909 then "Ragtime"
           when 1910..1929 then "New Orleans Jazz"
           when 1930..1939 then "Swing"
           when 1940..1950 then "Bebop"
           else "Jazz"
           end
    ```

* Use an empty line before the return value of a method (unless it
  only has one line), and an empty line between `def`s.

    ```Ruby
    def some_method
      do_something
      do_something_else

      result
    end

    def some_method
      result
    end
    ```

* Use RDoc and its conventions for API documentation.  Don't put an
  empty line between the comment block and the `def`.
* Use empty lines to break up a method into logical paragraphs.
* Keep lines fewer than 80 characters.
    * Emacs users might want to put this in their config
      (e.g. `~/.emacs.d/init.el`):

        ```
        (setq whitespace-line-count 80
              whitespace-style '(lines))
        ```

    * Vim
    * Textmate

* Avoid trailing whitespace.
    * Emacs users might want to put this in their config (ideally
      combine this with the previous example):

        ```
        (setq whitespace-style '(trailing space-before-tab
                                 indentation space-after-tab))
        ```

    * Vim users might want to put this in their `~/.vimrc`:

       ```
       autocmd BufWritePre * :%s/\s\+$//e
       ```

    * Textmate users might want to take a look at the [Uber Glory bundle](https://github.com/glennr/uber-glory-tmbundle).

## Syntax

* Use `def` with parentheses when there are arguments. Omit the
  parentheses when the method doesn't accept any arguments.

     ```Ruby
     def some_method
       # body omitted
     end

     def some_method_with_arguments(arg1, arg2)
       # body omitted
     end
     ```

* Never use `for`, unless you know exactly why. Most of the time iterators
  should be used instead.

    ```Ruby
    arr = [1, 2, 3]

    # bad
    for elem in arr do
      puts elem
    end

    # good
    arr.each { |elem| puts elem }
    ```

* Never use `then` for multi-line `if/unless`.

    ```Ruby
    # bad
    if some_condition then
      # body omitted
    end

    # good
    if some_condition
      # body omitted
    end
    ```

* Favor the ternary operator over if/then/else/end constructs.
  It's more common and obviously more concise.

    ```Ruby
    # bad
    result = if some_condition then something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

* Use one expression per branch in a ternary operator. This
  also means that ternary operators must not be nested. Prefer
  if/else constructs in these cases.

    ```Ruby
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* Never use `if x: ...` - it is removed in Ruby 1.9. Use
  the ternary operator instead.

    ```Ruby
    # bad
    result = if some_condition: something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

* Never use `if x; ...`. Use the ternary operator instead.

* Use `when x then ...` for one-line cases. The alternative syntax
  `when x: ...` is removed in Ruby 1.9.

* Never use `when x; ...`. See the previous rule.

* Use `&&/||` for boolean expressions, `and/or` for control flow.  (Rule
  of thumb: If you have to use outer parentheses, you are using the
  wrong operators.)

    ```Ruby
    # boolean expression
    if some_condition && some_other_condition
      do_something
    end

    # control flow
    document.saved? or document.save!
    ```

* Avoid multi-line ?: (the ternary operator), use `if/unless` instead.

* Favor modifier `if/unless` usage when you have a single-line
  body. Another good alternative is the usage of control flow `and/or`.

    ```Ruby
    # bad
    if some_condition
      do_something
    end

    # good
    do_something if some_condition

    # another good option
    some_condition and do_something
    ```

* Favor `unless` over `if` for negative conditions (or control
  flow `or`).

    ```Ruby
    # bad
    do_something if !some_condition

    # good
    do_something unless some_condition

    # another good option
    some_condition or do_something
    ```

* Never use `unless` with `else`. Rewrite these with the positive case first.

    ```Ruby
    #bad
    unless success?
      puts "failure"
    else
      puts "success"
    end

    #good
    if success?
      puts "success"
    else
      puts "failure"
    end
    ```

* Omit parentheses around parameters for methods that are part of an
  internal DSL (e.g. Rake, Rails, RSpec), methods that are with
  "keyword" status in Ruby (e.g. `attr_reader`, `puts`) and attribute
  access methods. Use parentheses around the arguments of all other
  method invocations.

    ```Ruby
    class Person
      attr_reader name, age

      # omitted
    end

    temperance = Person.new("Temperance", 30)
    temperance.name

    puts temperance.age

    x = Math.sin(y)
    array.delete(e)
    ```

* Prefer {...} over do...end for single-line blocks.  Avoid using {...} for
  multi-line blocks.  Always use do...end for "control flow" and "method
  definitions" (e.g. in Rakefiles and certain DSLs.)  Avoid do...end when
  chaining.

* Avoid `return` where not required.

    ```Ruby
    # bad
    def some_method(some_arr)
      return some_arr.size
    end

    # good
    def some_method(some_arr)
      some_arr.size
    end
    ```

* Avoid line continuation (\\) where not required. In practice, avoid using
  line continuations at all.

    ```Ruby
    # bad
    result = 1 - \
             2

    # good (but still ugly as hell)
    result = 1 \
             - 2
    ```

* Using the return value of = is ok.

    ```Ruby
    if v = array.grep(/foo/) ...
    ```

* Use ||= freely.

    ```Ruby
    # set name to Bozhidar, only if it's nil or false
    name ||= "Bozhidar"
    ```

* Avoid using Perl-style special variables (like $0-9, $`, ...).

* Never put a space between a method name and the opening parenthesis.

    ```Ruby
    # bad
    f (3 + 2) + 1

    # good
    f(3 + 2) + 1
    ```

* If the first argument to a method begins with an open parenthesis,
  always use parentheses in the method invocation. For example, write
`f((3 + 2) + 1)`.

* Always run the Ruby interpreter with the `-w` option so it will warn
  you if you forget either of the rules above!

## Naming

* Use `snake_case` for methods and variables.
* Use `CamelCase` for classes and modules.  (Keep acronyms like HTTP,
  RFC, XML uppercase.)
* Use `SCREAMING_SNAKE_CASE` for other constants.
* The names of predicate methods (methods that return a boolean value)
  should end in a question mark.
  (i.e. `Array#empty?`).
* The names of potentially "dangerous" methods (i.e. methods that modify `self` or the
  arguments, `exit!`, etc.) should end with an exclamation mark.
* The length of an identifier determines its scope.  Use one-letter variables
  for short block/method parameters, according to this scheme:

        a,b,c: any object
        d: directory names
        e: elements of an Enumerable
        ex: rescued exceptions
        f: files and file names
        i,j: indexes
        k: the key part of a hash entry
        m: methods
        o: any object
        r: return values of short methods
        s: strings
        v: any value
        v: the value part of a hash entry
        x,y,z: numbers

  And in general, the first letter of the class name if all objects are of
  that type.

* When using `inject` with short blocks, name the arguments `|a, e|`
  (accumulator, element).
* When defining binary operators, name the argument `other`.

    ```Ruby
    def +(other)
      # body omitted
    end
    ```

* Prefer `map` over *collect*, `find` over *detect*, `select` over
  *find_all*, `size` over *length*. This is not a hard requirement; if the
  use of the alias enhances readability, it's ok to use it.

# Author

* [bbatsov](https://raw.github.com/bbatsov/ruby-style-guide)
* community
