+++
author = "Mafinar Khan"
date = "2017-01-16T14:14:07+06:00"
title = "Day 3 - Mix"
linktitle = "Day 3 - Mix"
tags = [
    "mix"
]
weight = 10
description = "OMG It's over six months and I'm at day 3! Let's mix in some tooling today"
+++

## Hello Again
The past six month's been hectic at work and it's a pity I had little time for Elixir. And now version `1.4` is out, shame on me. 
This may not be the case anymore since I'm planning on hijacking some of Elixir greatness to my work mix.

And that's what I'm starting off after the hiatus... `Mix`

## Tools!
Every language comes encapsulated with a system of culture, tools and idioms. The tooling part is quintessential for developer productivity and happiness. It's really cool to see Elixir developers recognizing that part earlier on and building up a build tool which is known as `Mix`.

There's a lot of grunt work when kicking off with a project. A lot of meta-work is done for it starting right at the preparation phase. *What other libraries do I need?*, *What happens when I compile?*, *What about tests?*, *Works on MY MACHINE!*, *Where do modules live?* are few of the decision that needs love. That gave rise to a lot of tools that automate some of these, decide some of these, and lets you make your own (decisions/automation) in a frameworkish fashion. For `Clojure`, we have the amazing `Leiningen` (or the equally great `Boot`), for `Elixir`, we have `Mix`. And that's what I've played with (from) today, so that it in return allows *me* to play with the rest of the ecosystem in future.

## Hello Mix
The answer to those questions above is `Mix`. `Mix` is a build tool that lets you create, compile, dependancy-manage `Elixir` projects. 

Let's say we want to create a new `Elixir` project. We already have `Elixir` installed and `Mix` comes with it (thankfully). So all we need to do is, fire up the `mix` command and do something like `mix new HelloMix`. And it didn't work! This is because of how cool underscores and lower case letters are for folder names (see the cute little error message there?). Correcting ourselves: `mix new hello_mix` and it should work fine! 

Let's inspect the folder and see what `Mix` gave me-

```
.
├── .gitignore
├── README.md
├── config
│   └── config.exs
├── lib
│   └── hello_mix.ex
├── mix.exs
└── test
    ├── hello_mix_test.exs
    └── test_helper.exs
```

Well, without any preverification I could tell that `mix.exs` might just be `Mix`-s `project.clj` (i.e. `settings.py` :P), `test/` would contain the tests, `lib/` would house my modules, and with some verifications I got to know that `config/` contains all sorts of configuration maps of the modules I'd be using. I absolutely could not find out what `.gitignore` or `README.md` does though, might need to upgrade my google-fu.

Next step would be to inspect the `mix.exs` file. Here is it-

```
defmodule HelloMix.Mixfile do
  use Mix.Project

  def project do
    [app: :hello_mix,
     version: "0.1.0",
     elixir: "~> 1.4",
     build_embedded: Mix.env == :prod,
     start_permanent: Mix.env == :prod,
     deps: deps()]
  end

  # Configuration for the OTP application
  #
  # Type "mix help compile.app" for more information
  def application do
    # Specify extra applications you'll use from Erlang/Elixir
    [extra_applications: [:logger]]
  end

  # Dependencies can be Hex packages:
  #
  #   {:my_dep, "~> 0.3.0"}
  #
  # Or git/path repositories:
  #
  #   {:my_dep, git: "https://github.com/elixir-lang/my_dep.git", tag: "0.1.0"}
  #
  # Type "mix help deps" for more examples and options
  defp deps do
    []
  end
end
```

First it's the omnipresent `defmodule` *powered by* `use Mix.Project`. It pimps up the module with `Mix.Project` induced goodness. There are three sections at play here-

* **project** - I hold all the meta-information about what you're going to make. I decide how things get build, what `Elixir` you use, how to run things basically.
* **application** - I ensure what libraries need to get compiled before you compile your own?
* **deps** - I manage dependancies, and I'm private!

I think other than the `version` part, I should leave the `application` segment alone, for now. Let's play with time a little, there's a lib for that: [Timex](https://github.com/bitwalker/timex)

Common sense tells me to add a dependancy as a map, and it was correct, so let's add `{:timex, "~> 3.0"}` to our dependancy list so that it looks like:

```
  defp deps do
    [{:timex, "~> 3.0"}]
  end
```

I learnt more about versioning [from here](https://hexdocs.pm/elixir/Version.html). Well I would blindly copy the recommended line from the libraries for the time being any way. The comment in `mix.exs` code was kind enough to let me know of other possibilities (i.e. from git?). Thanks guys.

`mix deps.get` is the `npm install` of `Mix` it seems. Just did that and I intend to play with it in the REPL now. `iex -S mix` did the trick (`python manage.py shell` anyone?).

One thing I have seen is that I did not have to put anything in the `application` section of my Mix module, and it looked different from other `mix.exs`-es that I have seen, luckily I stumbled upon [this](https://sergiotapia.me/application-inference-in-elixir-1-4-ae9e43e90301#.mu8plww6p) which explained the reason and the application inference is really cool (although I have never lived a life before it). 

If I do a single `iex`, then I cannot get all the `deps` goodness I mentioned to `Mix`, the `-S mix` takes care of that and gives me an environment where `Time<TAB>` would autocomplete to `Timex`. So I can play with it straight away. Let's write something in `lib/hello_mix.ex`:

```
defmodule HelloMix do
  @moduledoc """
  Documentation for HelloMix.
  """

  @doc """
  Returns the ISO:Extended format of current time.
  """
  def now do
    {_, right_now} = Timex.format Timex.now(), "{ISO:Extended}"
    right_now
  end
end
```








