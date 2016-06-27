+++
author = "Mafinar Khan"
date = "2016-06-26T19:29:32+06:00"
title = "Day 0 - Unboxing and Exploration"
linktitle = "Day 0 - Unboxing and Exploration"
weight = 10
description = "Let's install this thing and do some exploratory programming!"
+++
## Get, Set, Go!!!
Obviously the first thing I need to do is, to unbox the goodies. For me, it's just `brew install elixir` and that's it. I will try out my Linux machine tomorrow. Moving on...

### The Editor
Vim's been good to me for the past decade and I see no reason why I should not use it. A quick googling introduced me to [alchemist.vim](https://github.com/slashmili/alchemist.vim) and [vim-elixir](https://github.com/elixir-lang/vim-elixir). I'm guessing the former does autocompletion and stuff while the latter is for syntax highlighting. I should have reversed the order maybe. 

I'm also liking Visual Studio Code a lot these days so might as well go and set it up too as well. I think the command I'm looking for is `ext install vscode-elixir`? Done. All set.

### The REPL
Here's the fun part. I like languages with REPLs, and those that encourage the use of it by giving some nice helpers built-in. After firing up the `iex` command, it seems to me that Elixir has awesome support for it. Just go ahead and try typing `h()` in the shell and you'll know what I mean.

One thing I noticed is the naming of the form `xxx/n` (i.e. `pwd/0`, `r/1`). A quick googling later, I see it's a function/arity pair Erlang guys use. **Does that mean no variadic argument for Elixir?**

Anyway, one of the things we Pythonistas are used to is the `help()` and `dir()` functions. Is there a way to do it in Elixir? The `h/0` and `h/1` are your friends for the former, and dot(.) + TAB for latter. So, if I do a `h(List)` (or `h List`) in the REPL, then I'd see good old documentation of `List`. Parentheses are optional for function calls, a little Ruby envy I always had. Awesome! But I still need to explore if I can get a list of methods of a particular module (That's what they call it?) like Python does with `dir()`. [Thank You StackOverflow](http://stackoverflow.com/questions/28664119/in-elixir-is-there-any-way-to-get-a-module-to-list-its-functions), I can get it by `<module>.__info__(:functions)` expression. Let's try: `List.__info__(:functions)`. Like a charm.

Any Vim user will know the value of exit command. `CTRL+C` does it for Elixir shell. That's exit!


### Exploration and Findings

#### Identifiers

Identifiers in Elixir

    + You can assign variables in Elixir. `number = 42` works, just as `country = "Bangladesh"` does. No `let`, `var`, `val` etc.
    + `f = 0.5` works but `f = .5` doesn't. There has to be something on the right side of `.`.
    + `PI = 3.1416` didn't work no matter what I tried. Variables shouldn't have the first letter capitalized [_why?_]
    + Atoms start with a colon (i.e. `:helloworld`)
    + I can use names like `integer?` or `map?` as identifiers. Yay! 

#### Functions
Erlang is a functional programming language. So it's only natural that I treat this at first. Now, how do I write a function in Elixir? 