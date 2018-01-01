---
title: Single letter variables
description: favouring typing over reading
---

Using variable / parameter names such as _a_, _b_, & _c_ remove all context except the position in arg list.  If the arguments are not semantically identical, then more descriptive names should be used.

## Example
When variables have a different __meaning__ then that should be reflected in the naming to make the code intention revealing.

```fsharp
let divide a b =
    a / b
  // or is it ...?
    b / a
```
```fsharp
let divide numerator denominator =
    numerator / denominator

    
```

## Exceptions
When the parameters are interchangeable then using nondescript names is appropriate, and in fact indicates to the reader that the name
is actually unimportant.

```fsharp
let sum a b c d =
    a + b + c + d
```

When the scope is a single line (or at least very small) and there is only one target parameter.  The single letter variable
indicates to the reader to focus on the transformation rather than the input.

```fsharp
let result = 
    inputList
    |> List.map    (fun x -> x * 2 + 37.4)
    |> List.filter (fun x -> x > 20)
```

## Refactorings
- Rename variable / parameter