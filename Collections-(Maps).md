## Maps

Another fundamental collection type is the **map**.
A map of type `Map[Key, Value]` is a data structure that associates keys of type `Key` with values of type `Value`.

```scala
val romanNumerals = Map("I" -> 1, "V" -> 5, "X" -> 10)                       // Map of type [String, Int]
val campitalOfCountry = Map("US" -> "Washington", "Switzerland" -> "Bern")   // Map of type [String, String]
```

### Maps = Iterables + Functions

Class `Map[Key, Value]` extends the collection type `Iterable[(Key, Value)]` Therefore, maps support the same collection operations as other iterables:
```scala
val countryOfCapital = capitalOfCountry map {
    case(x,y) => (y, x)
}
```
Note that maps extend iterables of key/value **pairs**. Infact, the syntax `key -> value` is just an alternative way to write the pair `(key, value)`

Class `Map[Key, Value]` extends the function type `Key => Value`, **so maps can be used everywhere functions can**. In particular, maps can be applied to key arguments. Eg.
```scala
capitalOfCountry("US") // returns "Washington"
```

### Querying Map using get
Applying a map to a non-existing key gives an error. What can we do to query a map without knowing if it contains a given key or not?

Instead of having a simple function application, we can call a `get` method on the map

```scala
capitalOfCountry("Andorra")   // Java NoSuchElementException error
```
So instead of this, we use `map get key` which returns an `Option` value:
```scala
captalOfCountry get ("Andorra")  // returns: Option[java.lang.String] = None
captalOfCountry get ("US")       // returns: Option[java.lang.String] = Some(Washington)
```

### The Option Type
```scala
trait Option[+A]
case class Some[+A](value: A) extends Option[A]
object None extends Option[Nothing]
```
An `Option` value can be one of two things, so the expression `map get key` either returns:
* `None`    : if `map` does not contain the given key
* `Some(x)` : if `map` associates the given `key` with the value x.

### Decomposing Option

Since options are defined as case classes, they can be decomposed using pattern matching:
```scala
def showCapital(country: String) = capitalOfCountry.get(country) match {
    case Some(capital) => capital
    case None => "missing data"
}
```
Options also support quite a few operations of the other collections. In particular, they support `map`, `flatMap`, and `filter`, so we can use them with `for-expressions`.

### Sorted and GroupBy

Two useful operations of SQL queries in addition to for-expressions are `groupBy` and `orderBy`

`orderBy` on a collection can be expressed with `sortWith` and `sorted`, which is just a "natural" ordering:

```scala
val fruit = List("apple", "pear", "orange", "pineapple")
fruit sortWith(_.length < _.length) //List("pear", "apple", "orange", "pineapple")
fruit.sorted //List("apple", "orange", 'pear", "pineapple")
```

`groupBy` partitions a collection into a `map` of collections according to a _discriminator function_. 

```scala
fruit groupBy (_.head)  // Map(p -> List(pear, pineapple), a -> List(apple), o -> List(orange))
```
As we know `head` is the first character that appears in each string. So `groupBy` gives us a `map` that maps head with a list of all fruits with that head.

### Map Example
A polynomial can be seen as a map from exponents to coefficients. For instance, `x^3 - 2x + 5` can be represented as `Map(0 -> 5, 1 -> -2, 3 -> 1)`.

Based on this observation, a class `Polynom` that represents polynomials as maps can be designed. 
```scala
class PolyNom(val terms: Map[Int, Double]) {
    def + (other: PolyNom) = PolyNom(terms ++ (other.terms map adjust)) // maps concatenated using ++ and wrapped as PolyNom. Then maps.adjust is called as the previous part only concatenates, does not add values with same coeffecients.
    def adjust(term: (Int, Double)): (Int, Double) = {
        val (exp, coeff) = term
        terms get exp match {
            case Some(coeff1) => exp -> coeff + coeff1
            case None => exp -> coeff
        }
    }
    override def toString() = {
         // (for( (exp, coeff) <- terms ) yield exp + "X^" + coeff) mkString "+"  // This prints fine, but in random order.
        (for( (exp, coeff) <- terms.toList.sorted.reverse ) yield exp + "X^" + coeff) mkString "+"
    }
}
```

### Default Values
So far, maps were **partial functions**: applying a map to a key value in `map(key)` could lead to an exception if the key was not stored in the map. What if we could make maps **total functions**, that would never fail but that would give back a default value if some key wasn't found?

The operation `withDefaultValue` turns a map into a total function:

```scala
val cap1 = capitalOfCountry withDefaultValue "<unknown>"
cap1("andorra")                  // returns: "unknown"
```

Hence we can change the above implementation of PolyNom as below:
```scala
class PolyNom(terms0: Map[Int, Double]) {
    val terms = terms0 withDefaultValue 0.0           // field = param withDefaultValue 0.0
    def + (other: PolyNom) = new PolyNom(terms ++ other.terms maps adjust) // maps concatenated using ++ and wrapped as PolyNom. Then maps.adjust is called as the previous part only concatenates, does not add values with same coeffecients.
    def adjust(term: (Int, Double)): (Int, Double) = {
        val (exp, coeff) = term
        // pattern match is now not needed now we don't need to test whether the given terms contain the given exponent or not
        exp -> (coeff + terms(exp))
    }
    override def toString() = {
         // (for( (exp, coeff) <- terms ) yield exp + "X^" + coeff) mkString "+"  // This prints fine, but in random order.
        (for( (exp, coeff) <- terms.toList.sorted.reverse ) yield exp + "X^" + coeff) mkString "+"
    }
}
```
Here, to allow the user to enter the polynomial in a simpler fashion, we can define auxilliary constructor:
```scala
def this(args: (Int, Double)*) = this(args.map) // * indicates repeated parameters
```
This can be used as:
```
val p1 = new PolyNom(Map(0 -> 5, 1 -> -2, 3 -> 1)) // original constructor
val p1 = new PolyNom(0 -> 5, 1 -> -2, 3 -> 1) // auxiliary constructor