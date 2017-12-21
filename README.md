# Welcome to Functional Code Smells

The listing of code smells as they pertain to functional programming.

## Code smells that are shared with Object Oriented programming 
Reference: https://blog.codinghorror.com/code-smells/

- [Comment](#comments)
- a
- b
- c

### Comments
> The best comment is an intention-revealing name
```csharp
DoSomething(int a, int b, int c) {
  // Triple the value
  var factor = 3.2 * c
  //Then add it
  return a * b + factor
}
```
```fsharp
fun do_something a b c =
  // Triple the value
  let factor = 3.2 * c
  //Then add it
  a * b + factor
```


## Code smells specific to Functional programmming 

- x
- y
- z

### Single letter variables
