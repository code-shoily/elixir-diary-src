+++
date = "2016-07-05T01:54:13+06:00"
author = "Mafinar Khan"
title = "Day 2 - Diving in..."
linktitle = "Day 2 - Diving In..."
tags = [
    "data-structure",
    "list",
    "tuple",
    "map",
    "comprehension",
]
weight = 10
description = "Since last night I've been playing with some of the data structures of Elixir, mostly lists, tuples and maps. While I'm not new to these data structures, the way Elixir's API is laid out is a little different and took awhile to get used to"
+++

Since last night I've been playing with some of the data structures of Elixir, mostly lists, tuples and maps. While I'm not new to these data structures, the way Elixir's API is laid out is a little different and took awhile to get used to. The syntaxes for list and tuple seemed intuitive enough, `[1, 2, [3, 4]]` is a list, `{91, 23}` is a tuple. With dictionary it was a little different, `%{"key" => "value"}` or `%{keyThatsAtom: "value"}`. Actually, that's just me remembering the feeling I had few days ago when I actually started Elixir. I already played a little with them, today I'll just get a better feeling of them since I've more or less grokked pattern matching (hopefully). 

### Tuple

I'll start with tuples. I first heard the term tuple while learning Python. In Elixir, a tuple is wrapped with curly braces and the syntax is like: `white = {255, 255, 255}`. The elements are place contiguously in memory. So it is faster for accessing tuples by index. There's a function for that too, `Kernel.elem/2`. `elem(white, 0)` will yield `255` (I should've picked another color). Since data is placed contiguously on memory, finding the length of the tuple is also easy, which is done with the `Kernel.tuple_length/1` function. `Kernel.is_tuple/1` tests for tuple, a pattern which we will see for other types as well. 

Tuples are fit for use cases that leverages its find-by-index performance issue and do not need the structure to grow, tuples don't like to grow or shrink, they did give a function for it that returns a grown up or shrunk down copy of them, they just don't like it. The are awesome for patterns, for example, or arguments to functions or return value too.

This is a pattern I've seen a lot in Elixir. You use tuples as return value and put the status as the first element of it, as an atom. It's kindof like the `(result, err)` pattern of `Golang`, with the `err` part being first, and as `keyword`, and mostly as `:ok`. They are also good candidates as function arguments and replacing if-s. For instance:

```elixir
defmodule Area do
    @pi 3.1416

    def compute({:square, side}) do
        side * side
    end

    def compute({:rectangle, length, breadth}) do
        length * breadth
    end

    def compute({:circle, radius}) do
        @pi * radius * radius
    end
end
``` 

### Lists
### Keyword Lists
### Maps
### Comprehensions
