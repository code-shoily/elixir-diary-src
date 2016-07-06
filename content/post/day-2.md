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

By the way, the `@pi` thing was *module attribute*, it has many purposes, one of them is to store module owned constants, and `@pi` was exactly that (I'll learn more on it later). 

### Lists

Lists in Elixir are linked lists. Each element has a value and a pointer to the next element. A list is of the type `[first_element | nextElementsAsList]` all the way till `nextElementsAsList` becomes `[]`. From this behavior it is clear that prepending to an Elixir list should be easier since it would mean getting the new element and putting the current list in the *tail section* (obviously, returning the new structure, not modifying the old list in anyway, immutable all the things!). 

If I got a nickel everytime I tried to access an element of a list by `list[element_index]` :( That doesn't work. If you want to access an element at a particular index, then you'll have to use the `Enum` module and use the `Enum.at/2` or `Enum.at/3` functions. Like I've mentioned earlier and I'll mention again as long as I don't get fully comfy with the pattern: "Elixir module functions are called like static methods, you do a `Module.function` call, put the data structure in question as the first argument, others as rest, there's no `x.y` where `x` is not a module (or as we'll see later, a map?)". So, `Enum.at [1,2,4], 1` yields `2` and `Enum.at [1,2,3], 10` yields `nil`. If you want to put a default value instead, then use the `Enum.at/3` function and the third argument is the default argument in case there's a `nil`.

> **Question** What if my list is `[1, nil, 3]` and I do a `Enum.at lst, 1`? Wouldn't it be better to raise an error instead of just `nil`? Or at least an `Enum.at!` alternative?

So what is this `Enum` thing? It does all the kung-fu required to play with iterables, want to `find` something? `map`, `reduce`, `filter`? `Enum`-s got it all. As an example, let's say we have a list of latitudes and longitudes of the path of a certain locomotive and we want to find the total distance travelled. So, the first thing we do is, extract `{lat, lng}` tuple from the list, then take 2 of each in `a->b, b->c, c->d` etc format, call the distance function amongst each pair, and then return the sum total of it. The operationset is as follows: 

* Define the distance function.
* Take a list, form a 2-tuple out of all the elements of the list
* Take chunks of adjacent tuples
* Compute the distance between each tuples of every chunk in the list
* Compute the total of all distances

```elixir
defmodule Distance do
    # Saving me some typings, `import` saves you from 
    # `<module>.` prefixes. Can be just `map` instead of `Enum.map`
    import :math # Erlang modules are written like :atoms
    import Enum

    @radius 6_371_000 # You can use underscores as large number separator. Neat!
    @pi :math.pi

    def square(x), do: x*x
    def to_radians(degree), do: degree / (180/@pi)

    def compute_diff {lat1, lng1}, {lat2, lng2} do
        {lat2 - lat1, lng2 - lng1} 
    end

    def compute [{lat1, lng1}, {lat2, lng2}] do
        {lat_diff, lng_diff} = compute_diff {lat1, lng1}, {lat2, lng2}
 
        a = square(sin(lat_diff/2)) + cos(lat1) * cos(lat2) * square(sin(lng_diff/2))
        c = 2 * asin(sqrt(a))
        
        @radius * c
    end

    def total(points \\ []) do
        points |> map(&to_radians/1)
               |> chunk(2)
               |> map(&List.to_tuple/1) 
               |> chunk(2, 1)
               |> map(&compute/1) 
               |> reduce(&+/2)
               |> (&//2).(1000)
    end
end
```

The `|>` is a super fun operator. It's work is simple, you take an expression, and pipe it through several `partial functions` through it, the expression result in question will be the first (missing) argument of the partial function, the result will be fed to the second one, and so one... so the snippet of `Distance.total/1` is actually the equivalent of `reduce(map(chunk(map(chunk(map(points, &to_radius/1), 2), &List.to_tuple), 2, 1), &compute/1), &+/2))/1000`. Phew! Each of the functions following the pipe command is actually missing a first argument... for instance, the `map(&List.to_tuple/1)` is really `map(?, &List.to_tuple/1)` where the `?` is to be filled up by the expression preceding it, the expressions keep evaulating and pillow passing the result to the next pipe until there's no more left, and that's when the combined result is expressed. It is a more `command centric` approach at processing and is very handy when it comes to transforming collections.

I have used `import`s so that I get saved from repetitively typing `Enum.map`, or `:math.sin`. I will look more into these later.

The `&` preceding what should've been a function body does exactly that. Expresses the function body. In `Elixir`, `f`, if not a variable, means call to the `f` function. So, had we written `map(list, List.to_tuple)`, the Elixir will attempt to call the `List.to_tuple/0` function, for which Elixir didn't define any. Also, it is important to mention the arity as well otherwise the compiler will get confused.
  

### Keyword Lists

Keywords lists are special lists in which all elements are 2-tuples in which first element's an atom. They have special syntax privileges. `[{:x, 10}, {:y, 20}]` can be conveniently typed as `[x: 10, y: 10]` and accessed as `kwl[:x]`.

Keyword lists add an additional syntactic benefit. If a named functions last argument is expected to be a keyword list, then the enclosing `[]` can be stripped off.

```elixir
defmodule StrippedKW do
    def example name: name, id: id do
        {:info, "#{name} - #{id}"}
    end
end
```
# And when calling...
StrippedKW.example name: name, id: id


### Maps
### Comprehensions
