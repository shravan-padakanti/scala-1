As seen previously, Scala exhibits polymorphism using: 

1. **Subtyping**:  Instances of subclass can be passed in place of a base class.
2. **Generics**: Classes or functions can accept generic type parameters.

Here we will look at their interaction:

* Bounds
* Variance

## Type Bounds

Consider the method `assertAllPos` which

* takes an IntSet
* returns the IntSet itself if all its elements are positive
* throws an exception otherwise

What would be the best type you can give to assertAllPos? Maybe:
```scala
def assertAllPos(s: IntSet): IntSet
```
In most situations this is fine, but can one be more precise?

One might want to express that `assertAllPos` takes Empty sets to Empty sets and NonEmpty sets to NonEmpty sets.
A way to express this is:
```scala
def assertAllPos[S <: IntSet](r: S): S = ...
```

Here, `<: IntSet` is an **upper bound** of the type parameter S: It means that S can be instantiated only to types that conform to IntSet. <br/>

### Upper Bounds
Generally, the notation.
* S <: T means: S is a subtype of T, and
* S >: T means: S is a supertype of T, or T is a subtype of S.

### Lower Bounds
You can also use a lower bound for a type variable.
Example: `S >: NonEmpty` introduces a type parameter S that can range only over supertypes of NonEmpty.
So S could be one of NonEmpty, IntSet, AnyRef, or Any.


