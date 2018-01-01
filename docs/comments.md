---
title: Comments
description: not just deodorant
---

Comments should explain _why_ a decision was made, not _what_ the code is doing.

```fsharp
let combine a b c =
    // Triple the factor
    let x = c * 3.2  // the super_factor
    // Then multiply each one
    (a + b) * x
```

```fsharp
let combine a b c =
    // from Fred's folio of foolproof factors, p55
    let super_factor = c * 3.2
    (a + b) * super_factor

```
