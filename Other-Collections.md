So far we have studied the Lists. Lists are *linear* i.e. access to the first element is much faster than the middle or the last element of the list. Here we will take a look at some other collections/sequences. One thing the collections have in common is that they are **immutable**.

# Collection Hierarchy

Common base class of `List`s and `Vector`s is `Seq` which itself is a subclass of `Iterable`. With `Seq` we also have `Set` and `Map`.

![Collections Hierarchy](collections_hierarchy)

**Arrays** and **Strings** : `Arrays` and `Strings` support the same operations as `Seq` and can implicitly be converted to sequences where needed. (They cannot be subclasses of Seq because they come from Java)
```scala
val xs: Array[Int] = Array(1, 2, 3)
xs map (x => 2 * x)
val ys: String = ”Hello world!”
ys filter (_.isUpper)
```

## Vector

A Vector is a data structure, which is an array for upto 32 elements. For more elements, this array becomes a array of pointers to 32 more arrays, thus it can store 1024 elements. Once this ies exhausted, we get an aray of pointers where each pointer points to another array of pointers which point to the elements. Thus the depth increases logarithmically: log32(N). Hence vectors has more uniform access patterns than lists.

![New and Old Vector](vector)

> When batch operations are recursive on the head, lists are preferred as they have constant time access. If batch operations are bulk on the rest of the list, vectors are preferred.

Vectors can be created as below:
```scala
val nums = Vector(1, 2, 3, -88)
val people = Vector(”Bob”, ”James”, ”Peter”)
```
They support the same operations as lists, with the exception of `::`. Instead of `x :: xs`, there is:
* `x +: xs` Create a new vector with leading element `x`, followed by all elements of `xs`.
* `xs :+ x` Create a new vector with trailing element x, preceded by all elements of `xs`.

(Note that the : always points to where the rest of the vector is.)

When we create a new vector using the above operation, since vectors are immutable, we create a new vector (red) and return it. The new vector uses elements from the old one (blue). Hence we have 2 vectors now which use common elements but don't interfere with each other.

![New and Old Vector](new_and_old)

## Ranges

Another simple kind of sequence is the range. It represents a sequence of evenly spaced integers.

Three operators:
`to` (inclusive), `until` (exclusive), `by` (to determine step value):
```scala
val r: Range = 1 until 5   // 1, 2, 3, 4
val s: Range = 1 to 5      // 1, 2, 3, 4, 5
1 to 10 by 3               // 1, 4, 7, 10
6 to 1 by -2               // 6, 4, 2
```
Ranges a represented as single objects with three fields: lower bound, upper bound, step value.