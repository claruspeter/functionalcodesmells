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
Having a deep hierarchy of values can lead to a developer to reason that is it simply easier to modify a component of the value in place, rather than to spend time/space of copying nearly the the entire structure.

```fsharp
type Planet = {name:string; order: int; distanceFromSun: double; mutable orbitalVelocity:double}
type SolarSystem = {name:string; relativePosition: Vector2; planets: Planet[]}
type Galaxy = {name: string; type:GalaxyType; position: Vector3; rotationalVelocity: double; systems: SolarSystem[]}
type Universe = {dimensionalOrder: int; galaxies: Galaxy[]}

let setPlanetSpeed universe nGalaxy nSystem nPlanet speed : unit =
    universe.[nGalaxy].[nSystem].[nPlanet].orbitalVelocity <- speed
```

Note that one of the indicators is that the code may contain a number of _setters_ for a large object.


```fsharp

```


## Refactorings & alternative designs
- [Lenses](https://medium.com/@haumohio/focusing-in-on-change-with-lenses)

## Related Smells
- Immutable Universe

## References
- <https://medium.com/@haumohio/handling-change-in-an-immutable-world>