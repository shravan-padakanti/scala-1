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

## Covariance

There’s another interaction between subtyping and type parameters we need to consider. 

Given:
```scala
NonEmpty <: IntSet
```
is
```scala
List[NonEmpty] <: List[IntSet] ?
```
Intuitively, this makes sense: A list of non-empty sets is a special case of a list of arbitrary sets.
We call types for which this relationship holds **covariant** because their subtyping relationship varies with the type parameter. Thus `Lists` in scala are **covariant**.

Does covariance make sense for all types, not just for List? No. In Scala Arrays are not Covariant.

In Java, Arrays are Covariant i.e. `NonEmpty[] <: IntSet[]`.

But this causes problems. Consider the Java code:
```java
NonEmpty [] a = new NonEmpty []{ new NonEmpty (1 , Empty , Empty )}
IntSet[] b = a
b[0] = Empty
NonEmpty s = a [0]
```
It looks like we assigned in the last line an `Empty` set to a variable of type `NonEmpty`! The 3rd line would give an `ArrayStoreException` as Java stores a _Type-tag_ for the Array against which it checks at Runtime.

**Question**: So when does it make sense to subtype one type with another?
**Answer**: **The Liskov Substitution Principle**: _If `A <: B`, then everything one can to do with a value of
type B one should also be able to do with a value of type A._

The above array example will be written in Scala as follows:
```scala
val a: Array[NonEmpty] = Array(new NonEmpty(1, Empty, Empty))
val b: Array[IntSet] = a
b(0) = Empty
val s: NonEmpty = a(0)
```
Scala gives a type error in line 2, since in Scala Arrays are **not covariant**.

### Details:

Lists are immutable and Arrays are mutable. So types which are mutable should not be covariant.

Say C[T] is a parameterized type, and A, B are types such that `A <: B`.

There are 3 possible relationships between C[A] and C[B]:
```
C[A] <: C[B]                                    // C is covariant
C[A] <: C[B]                                    // C is contravariant
neither C[A] or C[B] is a subtype of the other  // C is nonvariant
```
Scala lets you declare the variance of a type by annotating the type parameter:
```
class C[+A]
class C[-A]
class C[A]
```

So if 
```scala
type A = IntSet => NonEmpty
type B = NonEmpty => IntSet
```
According to the Liskov Principle, `A <: B`.