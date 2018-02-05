---
title: Mutable Variables
description: the inconsistant constant
---

One of the central tenants of functional programming is _immutability_.
Some of the benefits that have lead to embracing immutability include:
- no pass-by-ref accidental value changes
- automatic thread safety
- one name = one value within scope

So, if the software contains a __mutable__ variable then it may be a good time to ask why.
Most often, it is a symptom of a larger architecural problem and introducing a mutable variable could actually be solving the wrong problem, or even acting as an enabler of the wider problem.

## Example: 
Responding to a user interaction to update a property.  It may seem simplest to just update the property, but this can causes side-effects.  Now the model needs to be responsive to change, and may need to worry about things like multi-threaded access and race conditions.

The idiomatic response is to create a new, stable model with the new value in it.

```fsharp
type Planet = {
    name:string
    order: int
    distanceFromSun: double
    mutable orbitalVelocity:double
}

let setPlanetSpeed planet speed : unit =
    

let planet = universe.findPlanet('vulcan')
planet.orbitalVelocity <- speed
universe.recalcEverything()  // because something's changed







```

```fsharp
type Planet = {
    name:string
    order: int
    distanceFromSun: double
    orbitalVelocity:double
}

let planet = universe.findPlanet('vulcan')
let updatedplanet = {
    planet with orbitalVelocity = getValueFromUsr()
}
let updatedUniverse = {
    universe with planets = 
        planets |> map 
            (p->if p=planet then updatedplanet else p)
}
// carry on with updated universe
```

## Larger Example: Lenses
//TODO

## Refactorings & alternative designs
- Seeded constructor
- [Lenses](https://medium.com/@haumohio/focusing-in-on-change-with-lenses)

## Related Smells
- [Immutable Universe](immutable-universe)

## References
- <https://medium.com/@haumohio/handling-change-in-an-immutable-world>