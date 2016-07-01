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

What if I want to give them a default value of 0?

