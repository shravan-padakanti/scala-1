As seen previously, Scala exhibits polymorphism using: 

1. **Subtyping**:  Instances of subclass can be passed in place of a base class.
2. **Generics**: Classes or functions can accept generic type parameters.

Here we will look at their interaction:

* Bounds
* Variance

## Type Bounds

We have the class hierarchy `Any` > `AnyRef` > `IntSet` > `Empty` and `NonEmpty`

Consider the method `assertAllPositives` which

* takes an `IntSet`
* returns the `IntSet` itself if all its elements are positive
* throws an exception otherwise

What would be the best type you can give to assertAllPos? Maybe:
```scala
def assertAllPositives(s: IntSet): IntSet
```
In most situations this is fine, but can one be more precise?

Here if `assertAllPositives` takes Empty set, it returns Empty sets and  if it takes NonEmpty sets it returns NonEmpty sets. The above definition does not express this. A way to express this is using **bounds**:
```scala
def assertAllPositives[S <: IntSet](r: S): S = ...
```

Here, `<: IntSet` is an **upper bound** of the type parameter S: It means that S can be instantiated only to types that conform to IntSet. <br/>

### Upper Bounds
* `[S <: T]` means: S is a subtype of T.

Eg. If T is `IntSet`, then S could be one of `Empty`, `NonEmpty` or `IntSet`.

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
```scala
// Given
A <: B
// then:
C[A] <: C[B]                                    // C is covariant
C[A] >: C[B]                                    // C is contravariant
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
According to the Liskov Principle, `A <: B` - since B can return an Empty or NonEmpty, but A can return only NonEmpty.

So in general, functions are _contravariant_ in argument types and _covariant_ in return types:
```scala
// if
A2 <: A1 and B1 <: B2 
// then
A1 => B1 <: A2 => B2
```

### Practical Example:

```scala
trait List[T] {
    def isEmpty: Boolean
    def head: T
    def tail: List[T]
}

class Cons[T](val head: T, val tail: List[T]) extends List[T] { 
    def isEmpty = false
}

class Nil extends List[String] {
    def isEmpty: Boolean = true
    def head: T = throw new NoSuchElementException()
    def tail: List[T] = throw new NoSuchElementException()
}
```
Can be changed to:
```scala
trait List[+T] {
   ...
}

class Nil extends List[Nothing] { //Nothing type is at the bottom of Scala’s type hierarchy
   ...
}

//usage
val v : List[String] = Nil
```

# Understanding above

Reference: http://stackoverflow.com/questions/4531455/whats-the-difference-between-ab-and-b-in-scala

`Q[A <: B]` means that class Q can take any class A that is a subclass of B.

`Q[+B]` means that `Q` can take any class, but if `A` is a subclass of `B`, then `Q[A]` is considered to be a subclass of `Q[B]`.

`Q[-B]` means that `Q` can take any class, but if `A` is a subclass of `B`, then `Q[B]` is considered to be a subclass of `Q[A]`.

### Working example:

```scala
class Animal {}
class Dog extends Animal {}
class Poodle extends Dog {}

val animalsList: MYList[Animal] = new MYList(animal, dog) //polymorphism (parent class reference is used to refer to a child class object.)
val dogsList: MYList[Dog] = new MYList(dog)
```
1) Here `MYList` is nonvariant.
```scala
class MYList[T](elements: T*) {} 

def func1(list: MYList[Animal]) = {}
def func2(list: MYList[Dog]) = {}

func1(animalsList)  // GOOD
func1(dogsList)     // FAILS
func2(animalsList)  // FAILS
func2(dogsList)     // GOOD
```
2) Here `MYList` is covariant.
```scala
class MYList[+T](elements: T*) {} 

def func1(list: MYList[Animal]) = {}
def func2(list: MYList[Dog]) = {}

func1(animalsList)  // GOOD
func1(dogsList)     // GOOD - covariance
func2(animalsList)  // FAILS
func2(dogsList)     // GOOD
```
3) Here `MYList` is contra-variant.
```scala
class MYList[+T](elements: T*) {} 

def func1(list: MYList[Animal]) = {}
def func2(list: MYList[Dog]) = {}

func1(animalsList)  // GOOD
func1(dogsList)     // FAILS
func2(animalsList)  // GOOD - Contravariance
func2(dogsList)     // GOOD
```

### Another example

This shows that functions are _contravariant_ in argument types and _covariant_ in return types.

```scala
package com.example.playground

class Car {}
class SportsCar extends Car {}
class Ferrari extends SportsCar {}

object morecovariance extends App {

    // Test 1: Works as expected

    def test1( arg: SportsCar => SportsCar ) = {
        new SportsCar
    }

    def foo1(arg: Car): Ferrari = { new Ferrari }
    def foo2(arg: SportsCar): Car = { new Ferrari }
    def foo3(arg: Ferrari): Ferrari = { new Ferrari }

    test1(foo1) // compiles
    test1(foo2) // Fails due to wrong return type. 
    test1(foo3) // Fails due to wrong parameter type

}
```
