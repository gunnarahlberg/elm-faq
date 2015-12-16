---
title: Elm FAQ
layout: page
---
Here are some common questions that I've seen on the Elm IRC channel and on the mailing lists.
Contributions to [this document](https://github.com/fredcy/elm-faq) are welcome.

### Why does elm-repl (or elm-make) report "cannot find module 'Html'"?
You need to install the Html module:

    elm package install evancz/elm-html

Several modules are [available by default](http://package.elm-lang.org/packages/elm-lang/core/latest) in the base Elm tools but other common modules like Html have to be installed in the working directory before they can be used in elm-make, elm-repl, and elm-reactor.

### Why does elm-repl report a syntax problem for any type annotation, such as for `fib : Int -> Int`?

elm-repl does not support type annotations at all. If you try you'll see a "syntax problem" message saying something like "I ran into something unexpected when parsing your code!" and pointing to the colon.

To use type annotations you will have to use elm-reactor or [elm-lang.org/try](http://elm-lang.org/try), or build and run a complete app.

### How do I know what package name to use for `elm package install`?
Search on [package.elm-lang.org](http://package.elm-lang.org/) for the module name and use the package name that you find there.

### How can I write debugging information to the console?
Wrap any value with `Debug.log “some message”` and that message and the value will be written to the javascript console every time the value is evaluated. For example:

    case Debug.log "action" action of

If you want to just log a message and value without using that value, try this in a `let` clause:

    _ = Debug.log "my message" someValue

### How do I generate an Action as an Effect?

    Effects.task (Task.succeed SomeAction)

### How do I install an older version of Elm, 0.15.1 for example?

    npm install -g elm@0.15.1

If you need to switch between multiple versions of elm, consider [elmenv](https://github.com/sonnym/elmenv).

### What does `=>` mean?
A common idiom is to define the `(=>)` operator as a synonym for the `(,)` operator that constructs tuples. This makes a shorthand for long lists of tuple pairs, often used with the Html.style property.  So `["color" => "red", "padding" => "2px"]` means `[("color", "red"), ("padding", "2px")]`.

### How can I output literal HTML and avoid escaping of entities?
Use the `innerHTML` property. For example:

    span [ property "innerHTML" (Json.Encode.string "&copy;") ] []

### What does `()` mean?

It is the empty tuple or [unit type](https://en.wikipedia.org/wiki/Unit_type). It serves much like "void", "null", or "None" in other languages.

### What good is the `<|` operator if it is just function application?

It has lower precedence than function application expressed by adjacency (e.g. `sqrt x`) and so it can be used instead of parentheses to group function arguments. For example, `a b (c d)` is the same as `a b <| c d`.  More concretely, `max 3 (sqrt x)` can be written as `max 3 <| sqrt x`.

### What are the Elm operator precedences?

See an [Elm operator precedence table](operators.html).
See also [Basics.elm](https://github.com/elm-lang/core/blob/master/src/Basics.elm).

### Why isn't my StartApp-based program running any tasks?

You need to set `app.port`.

    port tasks : Signal (Task.Task Never ())
    port tasks =
        app.tasks

### Why doesn't the `<~` operator work?

It was removed in Elm version 0.16. You might use `andMap` from
[Signal.Extra](http://package.elm-lang.org/packages/Apanatshka/elm-signal-extra/5.7.0/Signal-Extra#andMap)
instead.

### How can I join the elmlang.slack.com community?
Sign up at [elmlang.herokuapp.com](http://elmlang.herokuapp.com/).
