---
title: Elm FAQ
layout: page
---

**This page is mostly for folks *learning* Elm.** It aggregates questions that are commonly
asked on [the Slack channel][slack], [the IRC channel][irc], or [the mailing list][list].
Those are all friendly and helpful places to go if you do not find the answer to your
question here!

[slack]: http://elmlang.herokuapp.com/
[irc]: http://webchat.freenode.net/?channels=elm
[list]: https://groups.google.com/forum/#!forum/elm-discuss

* Contributions to [this document](https://github.com/elm-community/elm-faq)
  are welcome!

* This document is about the current version of Elm (0.17). See also the
  [Elm 0.17 FAQ](17.html) about upgrading to Elm 0.17. See the [Elm 0.16 FAQ](16.html)
  about that older version of Elm.

#### Contents

<div id="elm-content"></div>


### What is the difference between `type` and `type alias`?

`type` defines and names a new type (which behaves like an enum with data
attached), and `type alias` gives a name to an existing type.

`type alias` isn't creating a distinct type, it is literally just giving a name to an existing type. A type alias will save you keystrokes, but do nothing more. [ref](https://groups.google.com/forum/#!topic/elm-discuss/YgRqI5s2S7Y)




### Why does elm-repl (or elm-make) report "cannot find module 'Html'"?
You need to install the Html module:

    elm package install elm-lang/html

Several modules are [available by default](http://package.elm-lang.org/packages/elm-lang/core/latest) in the base Elm tools but other common modules like Html have to be installed in the working directory before they can be used in elm-make, elm-repl, and elm-reactor.



### Why does elm-repl report a syntax problem for any type annotation, such as for `fib : Int -> Int`?

elm-repl does not support type annotations at all. If you try you'll see a "syntax problem" message saying something like "I ran into something unexpected when parsing your code!" and pointing to the colon.

To use type annotations you will have to use elm-reactor or [elm-lang.org/try](http://elm-lang.org/try), or build and run a complete app.



### How do I know what package name to use for `elm package install`?
Search on [package.elm-lang.org](http://package.elm-lang.org/) for the module name and use the package name that you find there.



### How can I write debugging information to the console?
Wrap any value with `Debug.log “some message”` and that message and the value will be written to the javascript console every time the value is evaluated. For example:

```haskell
case Debug.log "action" action of
```

If you want to just log a message and value without using that value, try this in a `let` clause:

```haskell
_ = Debug.log "my message" someValue
```

Also see [`Debug.crash`](http://package.elm-lang.org/packages/elm-lang/core/latest/Debug#crash)
which gets special treatment from the compiler to provide additional information in the output.



### How do I install an older version of Elm, 0.16 for example?

    npm install -g elm@0.16

If you need to switch between multiple versions of elm, consider [elmenv](https://github.com/sonnym/elmenv).



### What does `=>` mean?
A common idiom is to define the `(=>)` operator as a synonym for the `(,)` operator that constructs tuples. This makes a shorthand for long lists of tuple pairs, often used with the Html.style property.  So `["color" => "red", "padding" => "2px"]` means `[("color", "red"), ("padding", "2px")]`.

Note: Operators cannot be defined in elm-repl.



### What's the difference between `Html Msg` and `Html msg`?
`msg` is a placeholder used when the HTML doesn't send any messages of type `Msg` (as in `type Msg = ...`). This is just as `a` is used as a placeholder in `List a` when the list is of any type, rather than of strings (`List String`) or integers (`List Int`) etc.. `msg` means "message of any type".



### How can I output literal HTML and avoid escaping of entities?
Use the `innerHTML` property. For example:

```haskell
span [ property "innerHTML" (Json.Encode.string "&copy;") ] []
```



### What does `()` mean?

It is the empty tuple or [unit type](https://en.wikipedia.org/wiki/Unit_type). It serves much like "void", "null", or "None" in other languages.



### What good is the `<|` operator if it is just function application?

It has lower precedence than function application expressed by adjacency (e.g. `sqrt x`) and is right-associative, and so it can be used instead of parentheses to group function arguments. For example, `a b ( c d )` is the same as `a b <| c d`, and `f ( g ( h x ) ) )` can be written as `f <| g <| h x`.  More concretely, `max 3 (sqrt x)` can be written as `max 3 <| sqrt x`. [^application]

[^application]: Function application and the `<|` operator are discussed at some length [here](https://groups.google.com/d/msg/elm-discuss/-PLj_eamKVQ/Zzo7iNx2FgAJ).

Note: The `<|` operator is essentially the same as `$` in Haskell.



### What are the Elm operator precedences and associativities?

See an [Elm operator precedence table](operators.html).
See also [Basics.elm](https://github.com/elm-lang/core/blob/master/src/Basics.elm).



### How can I use multiple Elm programs on the same page?

You can compile multiple modules into a single elm.js and then instantiate whatever module you need on the appropriate div. [^multipleModules]

    elm-make A.elm B.elm --output elm.js

[^multipleModules]: Use of multiple main modules in one application is discussed [here](https://groups.google.com/d/msg/elm-discuss/eEJgNnl99ps/keWXnn1KCwAJ).


### Does the main module file have to be named "Main.elm"?

No, that is just a convention. Any module containing a `main` function of type `Program Never` can be an entry point to an Elm program.

For example, if both Foo.elm and Bar.elm contain an appropriate `main` function,
compiling via "elm-make Foo.elm Bar.elm \-\-output elm.js" creates an elm.js file
such that both `Elm.Foo.embed(someElement)` and
`Elm.Bar.embed(someOtherElement)` can be used from the same HTML file.


### Why doesn't the Elm compiler find the Native code in a module that I cloned from github?

To use native code not installed by `elm-package` you need to add this to your elm-package.json file:

    "native-modules": true,



### Why, when I import a module that defines a type, does the compiler know about the type name but not its constructors?

You need to import the module in one of the following ways:

    import MyModule exposing (MyType(..)) -- exposes all constructors
    import MyModule exposing (MyType(MyConstructor)) -- exposes only MyConstructor

Just exposing `MyType` without the `(..)` will leave the constructors undefined, while exposing `(MyConstructor)` will expose `MyConstructor` but hide `AnotherConstructor`.

Similarly, the module itself must export the constructors.

    module MyModule exposing (MyType(..)) -- exposes all constructors
    module MyModule exposing (MyType(MyConstructor)) -- exposes only MyConstructor

However, there are reasons for [keeping tags and record constructors secret](http://package.elm-lang.org/help/design-guidelines#keep-tags-and-record-constructors-secret).

You can also choose to export a type without exporting any constructors like this (just make sure you expose some other way of creating values of your type):

    module MyModule exposing (MyType, myOtherFunctions)



### Where can I use type annotations?

In addition to the top-level, type annotations can also be applied to `let` expressions.

```haskell
let
  hypotenuse : Float -> Float -> Float
  hypotenuse a b =
    sqrt (a^2 + b^2)
in
  hypotenuse 3 4
```



### How can I join the elmlang.slack.com community?
Sign up at [elmlang.herokuapp.com](http://elmlang.herokuapp.com/).



### How can I recover when elm-make fails with errors like "... openFile: does not exist"?

That can happen when switching between elm versions. Try removing all of elm-stuff or just the build-artifacts:

    rm -r elm-stuff/build-artifacts



### How do I install an Elm package that has not been published to packages.elm-lang.org for use in my project?

Clone the package into a separate directory and add its directory path to the `source-directories` section of the elm-package.json file for your project. As usual, you will also have to install any dependencies of the package. If the package includes any native javascript code you will have to also add `native-modules: true` to elm-package.json.



### How can I parse Json into Elm data?

Currently you have to write the [parsing
code](http://package.elm-lang.org/packages/elm-lang/core/latest/Json-Decode).
Other than for [data passed over ports](http://elm-lang.org/guide/interop#customs-and-border-protection) there is no automatic conversion (and even
there, experts recommend writing the parser manually to be able to handle error
cases).

The [json-to-elm](https://github.com/eeue56/json-to-elm) tool is a Python script that generates Elm code to parse a given example of Json. That tool is available online at [json2elm.com](http://json2elm.com).

There are additional Json parsing tools in the [elm-json-extra](http://package.elm-lang.org/packages/circuithub/elm-json-extra/latest/Json-Decode-Extra) package.

The [Elm Json Decode interpreter](http://simonh1000.github.io/decoder/) is an online tool allowing you to experiment with decoders and test them in a simple context.


### How can I report a compiler error message that could be better?

Report the problem at the [error-message-catalog issue tracker](https://github.com/elm-lang/error-message-catalog/issues)
with an [short, self-contained, correct, example](http://sscce.org/) showing both the program and the problematic error messages.



### Does Elm have HashMaps?

The core [Dict](http://package.elm-lang.org/packages/elm-lang/core/latest/Dict) package provides a dictionary mapping unique keys to values. There are some restrictions on key value types; in particular, records cannot be keys.


### Why does my app fail immediately with a console error of "Uncaught TypeError: Cannot read property 'appendChild' of null"?

Make sure that you are calling Elm's javascript `embed` function *after* the
referenced container has been defined in the HTML file.

Similarly, Elm's `fullscreen` function should be called only after the page body has started.

Good practice is to call `embed` or `fullscreen` at the end of the document body.


### How can I load CSS (or other resources) in elm-reactor?

That is not currently possible because elm-reactor now serves other files as plaintext.
Consider using other hot-reload tools such as
[elm-live](https://github.com/tomekwi/elm-live),
[elm-hot-loader](https://github.com/fluxxu/elm-hot-loader) (for use with webpack),
or [devd](https://github.com/cortesi/devd).

The 0.17.1 release of Elm fixes this problem; elm-reactor serves resources such as CSS and HTML files correctly again.


### How does one render an HTML node conditionally?

Use `Html.text ""` as an empty element. E.g.

```haskell
if someCondition then
    Html.div [] [ {- ... some substantial Html value here ... -} ]
else
    Html.text ""
```

## Footnotes

