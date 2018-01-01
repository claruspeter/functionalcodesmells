# Single letter variables
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

When the parameters are interchangeable then using nondescript names is appropriate, and in facts indicates to the reader that the name
is actually unimportant.

```fsharp
let sum a b c d =
    a + b + c + d
```

## Refactorings
- Rename variable / parameter