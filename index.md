---
title: Elm FAQ
layout: page
---

### Why does elm-repl (or elm-make) report "cannot find module 'Html'"?
You need to install the Html module:

    elm package install evancz/elm-html

### Why does elm-repl report a syntax problem for any type annotation, such as for `fib : Int -> Int`?

elm-repl does not support type annotations at all.

### How do I know what package name to use for `elm package install`?
Search on [package.elm-lang.org](http://package.elm-lang.org/) for the module name and use the package name that you find there.

### How can I write debugging information to the console?
Wrap any value with `Debug.log “some message”` and that message and the value will be written to the javascript console every time the value is evaluated. For example:

    case Debug.log "action" action of

### How do I generate an Action as an Effect?

    Effects.task (Task.succeed SomeAction)

### How do I install an older version of Elm, 0.15.1 for example?

    npm install -g elm@0.15.1

If you need to switch between multiple versions of elm, consider [elmenv](https://github.com/sonnym/elmenv).

### What does `=>` mean?
A common idiom is to define the `(=>)` operator as a synonym for the `(,)` operator that constructs tuples. This makes a shorthand for long lists of tuple pairs, often used with the Html.style property.  So `["color" => "red", "padding" => "2px"]` means `[("color", "red"), ("padding", "2px")]`.

### How can I output literal HTML and avoid escaping of entities?
Use the `innerHTML` property:

    span [ property "innerHTML" (Json.Encode.string "&copy;") ] []

### How can I join the elmlang.slack.com community?
Sign up at [elmlang.herokuapp.com](http://elmlang.herokuapp.com/).
