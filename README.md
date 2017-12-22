With the expansion and acceptance of functional programming in the commercial world certain patterns and anti-patterns are emerging.

This is an effort to document a comprehensive list of code smells that can appear in functional code.

## Code smells specific to Functional programmming 
These code smells are either unique to functional programming, or have a particular emphasis in the functional programming community.

### Single letter variables
Using variable / parameter names such as _a_, _b_, & _c_ remove all context except the position in arg list.  If the arguments are not semantically identical, then more descriptive names should be used.

#### Example
This is OK:

```fsharp
let add a b =
    a + b
```

This is not:

```fsharp
let divide a b =
    a / b
  // or is it ...?
    b / a
```

When variables have a different __meaning__ then that should be reflected in the naming to make the code intention revealing.


```fsharp
let divide numerator denominator =
    numerator / denominator
```

#### Refactorings
- Rename variable / parameter

### Record of functions
### Mixed code and data
### Functions with no output

References:
- [Scott Wlaschin- code refactor](https://www.youtube.com/watch?v=nxIRlf4AtcA)

## Code smells that are shared with Object Oriented programming 
These code smells are nothing new and generally apply to functional programming just as much as they apply to object oriented programming.  For more details of what these smells are, and what to do about them, please see the references.

- Comments
  - Bad:
```fsharp
    let combine a b c =
        // Triple the factor
        let x = c * 3.2  // the super_factor
        // Then multiply each one
        (a + b ) * x
```
  - Good:
```fsharp
    let combine a b c =
        let super_factor = c * 3.2 // from Fred's folio of foolproof factors, p55
        (a + b ) * super_factor
```
- Duplicate code
- Dead code
- Speculative Generality
- Shotgun surgery
- Long function
- Large module
- Primitive Obsession
- Long parameter list
- Data clumps
- Middle man
- Message chains
- Oddball
- Switch statement
- Conditional complexity
- Combinatorial Explosion
- Temporary field
- Side effect


References: 
 - <https://blog.codinghorror.com/code-smells/>
 - <https://refactoring.guru/refactoring/smells>


