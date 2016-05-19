---
title: Elm 0.17 Tasks
layout: page
---

Here are some ways to convert Elm 0.16 code that uses `Effects.task` into Elm 0.17.

The cases are sorted by priority, so use the first one that applies to your code.

<table class="task-conversions">
    <tr>
	<th>Elm 0.16 example</th>
	<th>Elm 0.17 equivalent</th>
    </tr>
    <tr>
	<td><code>task |> Task.toMaybe |> Effects.task</code></td>
	<td><code>task |> Task.perform (always Nothing) Just</code></td>
    </tr>
    <tr>
	<td><code>task |> Task.toResult |> Effects.task</code></td>
	<td><code>task |> Task.perform Err Ok</code></td>
    </tr>
    <tr>
	<td><code>task |> Task.map action |> Effects.task</code></td>
	<td><code>task |> Task.perform never action</code></td>
    </tr>
    <tr>
	<td><code>task |> Effects.task</code></td>
	<td><code>task |> Task.perform never identity</code></td>
    </tr>
</table>


The function `never` used in two places above is from [elm-community/basics-extra](http://package.elm-lang.org/packages/elm-community/basics-extra/latest).

This recommendation comes from an
[elm-discuss posting](https://groups.google.com/d/msg/elm-discuss/gkdCrioDsUQ/cJn5-n6fFQAJ)
by Janis Voigtländer on 2016-05-19.


	    
	
