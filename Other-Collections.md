So far we have studied the Lists. Lists are *linear* i.e. access to the first element is much faster than the middle or the last element of the list. Here we will take a look at some other collections/sequences. One thing the collections have in common is that they are **immutable**.

## Vector

A Vector is a data structure, which is an array for upto 32 elements. For more elements, this array becomes a array of pointers to 32 more arrays, thus it can store 1024 elements. Once this ies exhausted, we get an aray of pointers where each pointer points to another array of pointers which point to the elements. Thus the depth increases logarithmically: log32(N). Hence vectors has more uniform access patterns than lists.

> When batch operations are recursive on the head, lists are preferred as they have constant time access. If batch operations are bulk on the rest of the list, vectors are preferred.

Vectors can be created as below:
```scala
val nums = Vector(1, 2, 3, -88)
val people = Vector(”Bob”, ”James”, ”Peter”)
```
They support the same operations as lists, with the exception of `::`. Instead of `x :: xs`, there is:
* `x +: xs` Create a new vector with leading element `x`, followed by all elements of `xs`.
* `xs :+ x` Create a new vector with trailing element x, preceded by all elements of xs.
(Note that the : always points to the sequence.)



