+++
date = "2016-07-02T01:54:13+06:00"
author = "Mafinar Khan"
title = "Day 1 - Tabula Rasa"
linktitle = "Day 1 - Tabula Rasa"
weight = 10
description = "I did enough exploration the past few days and it's time I clean slate and proceed as someone learning Elixir. Not a Python programmer trying to translate Python idioms into Elixir or a Clojure programmer's view of the language, but starting with a clean slate and learning from zero."
+++

I did enough exploration the past few days and it's time I clean slate and proceed as someone learning Elixir. Not a Python programmer trying to translate Python idioms into Elixir or a Clojure programmer's view of the language, but starting with a clean slate and learning from zero. It is not an easy thing to do. Most of the time I would feel the urge to skip a paragraph of the documentation with the assumption that "I already know". I don't. My exposure to Elixir is that of a pre-noob's and I should act like it.

Although it's entitled *"Day 1"*, I had spent the past few days reading through the documentation and browsing through some source codes, I needed to get a feel for the language before I started documenting my experiences with it. While from a distance, Elixir seemed to have a familiar face but in reality, the way it makes you think and the way the codes become art is quite different from others. The same is true for Python, or Golang, or Clojure. Every language boasts their own personality and it is important that their coders respect that. Writing Fortran in any language never did any good.

So, assuming zero knowledge of the language, I proceed with the rest of my days with Elixir.

How do I run a file? I know `iex` is the magical shell that let's me insta-code, but what if I want to write a program, a script of sorts with Elixir? Easy, I write a `.exs` file and do an `elixir <my_file_name>.exs`. 

```elixir
# hello_world.exs
defmodule HelloWorld do
    def run do
        IO.puts "Hello World"
    end
end

HelloWorld.run
```

There you go, my first super sophisticated Elixir program. A quick `elixir hello_world.exs` printed the legendary "Hello World" to my console. I could go to the shell and type `c("hello_world.exs")` and enable it for a `HelloWorld.run` too if I want.

What if I am in the shell, compiled the code and then I change it and want to see the results? Just make the `c` call again. 

*Modules* in Elixir contain functions. There are anonymous functions which are of the `fn(<arg1>,<arg2>...) -> <body> end` syntax and can be bound with a var, but for the named functions to exist, they have to reside under a module. 

```elixir
# Anonymous function
square_area = fn(n) -> n*n end

# Inside a module
defmodule Area do
    def square(side) do
        side * side
    end
end
```

We can add more functions to the module, for instance, the ability to compute the area of a rectangle, given `height` and `width`:

```elixir
defmodule Area do
    def square(side \\ 0) do
        side * side
    end

    def rectangle(height \\ 0, width \\ 0) do
        height * width
    end
end
```

What were those `\\ 0` about? That's how Elixir shows default arguments. `height \\ 0` and `width \\ 0` meant that if any of those parameters were missing during function call, they'd be replaced with `0`s. I think I will typo `//` instead of `\\` a lot.

The `=` in Elixir is called a *match operator*. It doesn't seem like other `=`-s in a sense that it doesn't assign anything to a variable, it matches, and binds the right hand side expression with the left hand side ones. 

```elixir
x = 1 # We all know what happens here
x = 2 # `x` now refers to someone else
3 = x # 3 does *not* match with `x`, so we get a `MatchError`
2 = x # There it is, RHS matches with LHS, so it works!
```

I always be sure to familiar myself with error messages of a programing language. `2 = x` yielded no error but `3 = x` did spit up a `MatchError`, so naturally, so should `3 = z`, as `z` doesn't exist to be matched in the first place. However, this will yield a completely different error- the `CompileError`. It's because the compiler expect the right hand side to be an expression which the left hand side will match with. `x` is an expression, it didn't match with `3`. `z` isn't, so it took it for a function (it looks like a no-args function since parentheses are optional in Elixir), so the error message is, `undefined function, z/0`.

This brings us to exception handling. I know it's too early to be handling exceptions but well, I dive in there early. Let's start with everybody's favorite example:

```elixir
# Let's 1/0 and see what the error is called, ArithmeticError eh?
defmodule BadMath do
    def divide(a, b) do
        try do
            a / b
        rescue
            e in ArithmeticError -> IO.puts("An error occured: " <> e.message)
        after
            IO.puts "I just print this message for no reason at all"
        end
    end

    def play do
        IO.puts divide(10, 0)
    end
end
```

**Question** What does the `<>` operator do?
**Answer** That's Elixir string concatenation operator. (I felt kinda weird to me at first)

When we either run the script or call `BadMath.play` from `iex` we get the error message and the ~~finally~~ `after` message. So I guess we can tell that `after` puts a piece of code that gets executed regardless whether you made an error or not.

How do you ~~throw~~ `raise` an exception? The `raise` macro, of course. Just put `raise` with the ErrorName as the first argument and message as the second. Call it in `try`-s scope and it'll ~~catch~~ `rescue` it.

Speaking of striking out things, looks like Elixir has `throw` and `catch` as well (See what I mean by starting with _No knowlege at all?_). `throw` is *not* like `raise`. It just does it's namesake, takes a value, and throws it into the `catch`ers pitch, anything that is thrown gets handled. Like the following:

```elixir
tct = fn a,b ->
    try do
        c = a/b
        throw c
    rescue
        e in ArithmeticException -> IO.puts "Know thy Arithmetic"
    catch
        v when v <= 10 -> IO.puts "Small result: #{v}"; v
        v when v > 10 == 1 -> IO.puts "Large result: #{v}"; v
        v -> IO.puts "Is it even possible to get here?"
end
```
**Question** `#{v}`? Does it do what I think it does?
**Answer** Yes. The braces take Elixir expression, evaluates, stringifies, and replaces the `#{}` with it. It's called *String Interpolation*.

**Question** What's with the `->` and the `when`?
**Answer** Bare with me, I'll get there ~~tomorrow?~~ right after this. It's readable though, right?

One thing we notice is, a lot of functions demand to be wrapped inside a `try`. And there's sugar for it too:

```elixir
defmodule BadMath do
    def divide(a, b) do
        a/b
    rescue
        e in ArithmeticError -> IO.puts("Oops: #{e.message}")
    after
        IO.puts("I get printed regardless of whether you screwed up")
    end
end
```
So a named function can be an implicit `try` too.

Here's the takeaway:

* `try` creates somewhat of a protected zone around your code. Any ~~Exception~~ errors occuring or `raise`d becomes a candidate for `rescue`.
* The `rescue` clause takes the error in question and matches it with a list of patterns (We're getting there soon). Whenever a pattern is matched, the associated expression is run.
* The `catch` clause deals with stuff that `try` threw at it via the `throw` function. It too, has it's own set of pattern against which the thrown value is matched with, it is better for the patterns to be exhaustive here. 
* The difference between `raise` and `catch` is that `raise` is activated when an error occures, it knows the `error` and its `message`. `catch` on the other hand is activated when something is `throw`n at it. It can be *any* value the author chooses it to be. And the value will be matched against a series of pattern, just like the `rescue` clause.
* Named functions can act as an *implicit* try. If the whole body of the function is to be probed for error, then the try wouldn't be required.
* `after` takes place regardless of whether the error was made
* `else` takes place when no errors were raised or values were thrown. It also matches the value with a series of patterns just like `raise` and `catch`, it just takes on the good guys. (I had forgotten about it earlier and am too lazy to be showing an example now :()

