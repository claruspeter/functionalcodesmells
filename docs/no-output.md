---
title: Functions with no output
description: anybody there?
---

A function _should_ work within a bounded context and have no side effects.
Therefore, if a function doesn't return a value it is probably interacting with something other than the provided parameters.

This leads to two main problems:
1. Side effects - where it is impossible to tell what the function interacts with from outside the function's code.
2. Composibility - Outer functions end up being a series of steps (like an imperative language), rather than a set of functions that feed data along a pipeline.

## Example: I/O
Often the reason for this is that a function is doing writing to / reading from the "outside world".

In this example, the function interacts with a global static class that prints to the screen, but it could just as easily be writing to a file or doing absolutely nothing.  The solution to this is to pass in the external party as a parameter to the function

```fsharp
let sayHelloTo name : unit =
    Console.Writeline("Hello " + name)

sayHelloTo "Me"




----
Hello Me

----

>> it: unit
```

```fsharp
let sayHelloTo name console : TextWriter =
    console.Writeline("Hello " + name)
    console

 aConsoleWriter
    |> sayHelloTo "Me"
    |> sayGoodByeTo "You"

----
Hello Me
Goodbye You
----

>> it:ConsoleWriter = ["Hello Me"; "Goodbye You"]
```


## Example: Side effects
A less palatable reason is that the function is interacting with wide-scoped (and possibly mutable) variable, often in order to provide an appearance of persistence.

Again, the solution is to pass in the data to the function as a parameter and deal with the data as it's own instance.  If you feel you need to keep the contents of the data structure private then you can use signature files (*.fsi) to define the external visibility.

```fsharp
module MyModule

mutable private secret = 42
mutable private lastEdited = ""


let goOneBetter byWhom : unit =
    secret <- secret + 1
    lastEdited <- byWhom


```

```fsharp
module MyModule

type MyData = { value: int; lastEdited: string}
              static member Zero = 
                  {value = 42; lastEdited = ""}

let goOneBetter byWhom (data:MyData) : MyData =
    {
        value = data.value + 1
        lastEdited = byWhom
    }
```

## Refactorings
- Extract Parameter

## Related Smells
- Side effect
- Mutable variables
- Mixed code and data
