## Maps

Another fundamental collection type is the **map**.
A map of type `Map[Key, Value]` is a data structure that associates keys of type `Key` with values of type `Value`

```scala
val romanNumerals = Map("I" -> 1, "V" -> 5, "X" -> 10)
val campitalOfCountry = Map("US" -> "Washington", "Switzerland" -> "Bern")
```

### Maps = Iterables + Functions

Class `Map[Key, Value]` extends the collection type `Iterable[(Key, Value)]`
Therefore, maps support the same collection operations as other iterables:
```scala
val countryOfCapital = capitalOfCountry map {
    case(x,y) => (y, x)
}
```

Note that maps extend iterables of key/value **pairs**. Infact, the syntax `key -> value` is just an alternative way to write the pair `(key, value)`

Class `Map[Key, Value]` extends the function type `Key => Value`, **so maps can be used everywhere functions can**. In particular, maps can be applied to key arguments. `capitalOfCountry("US")` is a well-formed application; looks like a function call which gives us back "Washington."