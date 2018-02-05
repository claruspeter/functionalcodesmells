---
title: Immutable Universe
description: locked in perfection
---

One of the central tenants of functional programming is _immutability_.
Some of the benefits that have lead to embracing immutability include:
- no pass-by-ref accidental value changes
- automatic thread safety
- one name = one value within scope

Having a deep hierarchy of values can lead to a developer to reason that is it simply easier to modify a component of the value in place, rather than to spend time/space of copying nearly the the entire structure.

## Example: 

```fsharp
type Planet = {name:string; order: int; distanceFromSun: double; mutable orbitalVelocity:double}
type SolarSystem = {name:string; relativePosition: Vector2; planets: Planet[]}
type Galaxy = {name: string; type:GalaxyType; position: Vector3; rotationalVelocity: double; systems: SolarSystem[]}
type Universe = {dimensionalOrder: int; galaxies: Galaxy[]}

let setPlanetSpeed universe nGalaxy nSystem nPlanet speed : unit =
    universe.[nGalaxy].[nSystem].[nPlanet].orbitalVelocity <- speed
```

But this is BAD &tm;, because of the [mutable variable smell](mutable-variables.md).  So an common solution is to use the lens pattern, which is a set of functions that effectively get and set values from structures in an immutable fashion.

```fsharp
// ...all of the above types without the mutable plus...
let PlanetSpeed Lens<Planet, double> =
  { Get = fun (p: Planet) -> p.orbityVelocity
    Set = fun v (x: Planet) -> { x with OrbitalVelocity = v } }
let PlanetOrder<Planet, int> // as above ...
let PlanetDistance<Planet, double>

let SystemPlanets<SolarSystem, Planet[]>
let GalaxySystems<Galaxy, SolarSystem[]>
let Galaxies<Universe, Galaxy[]>

//....

let lens = 
    Galaxies 
    >>| nth 2 
    >>| GalaxySystems 
    >>| nth 42 
    >>| SystemPlanets 
    >>| nth 3 
    >>| PlanetSpeed
universe |> lens.set 73.465
```

This is a nice, regular pattern that allows the developer to compose a lens function that can burrow through a large hierarchy.  However, each property of each type needs an explicit lens function, which has the effect of concreting the incumbent solution leading to a __higher cost of change__.

## Example: break down components
//TODO


## Refactorings & alternative designs
- [Lenses](https://medium.com/@haumohio/focusing-in-on-change-with-lenses)

## Related Smells
- [Mutable Variables](mutable-variables)

## References
- <https://medium.com/@haumohio/handling-change-in-an-immutable-world>