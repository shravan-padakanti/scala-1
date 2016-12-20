As seen previously, Scala exhibits polymorphism using: 

1. **Subtyping**:  Instances of subclass can be passed in place of a base class.
2. **Generics**: Classes or functions can accept generic type parameters.

Here we will look at their interaction:

* Bounds
* Variance

## Type Bounds

We have the class hierarchy `Any` > `AnyRef` > `IntSet` > `Empty` and `NonEmpty`

Consider the method `assertAllPos` which

* takes an IntSet
* returns the IntSet itself if all its elements are positive
* throws an exception otherwise

What would be the best type you can give to assertAllPos? Maybe:
```scala
def assertAllPos(s: IntSet): IntSet
```
In most situations this is fine, but can one be more precise?

Here if `assertAllPos` takes Empty set, it returns Empty sets and  if it takes NonEmpty sets it returns NonEmpty sets. The above definition does not express this. A way to express this is using **bounds**:
```scala
def assertAllPos[S <: IntSet](r: S): S = ...
```

Here, `<: IntSet` is an **upper bound** of the type parameter S: It means that S can be instantiated only to types that conform to IntSet. <br/>

### Upper Bounds
* `[S <: T]` means: S is a subtype of T.

Eg. If T is `IntSet`, then S could be one of `NonEmpty` or `IntSet`.

### Lower Bounds
* `[S >: T]` means: S is a supertype of T, or T is a subtype of S.

Eg. If T is `NonEmpty`, then S could be one of `NonEmpty`, `IntSet`, `AnyRef`, or `Any`.

### Mixed Bounds
* `[S >: T2 <: T1]` means: s is any type on interval between T1 and T2.

Eg. if T2 is `NonEmpty` and T1 is `IntSet`, then S is any type on the interval between `NonEmpty` and `IntSet.

### Covariance

