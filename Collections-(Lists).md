The list is a fundamental data structure in functional programming. A list having `x1, ..., xn` as elements is written `List(x1, ..., xn)`.

Example
```scala
val fruit = List(”apples”, ”oranges”, ”pears”)
val nums = List(1, 2, 3, 4)
val diag3 = List(List(1, 0, 0), List(0, 1, 0), List(0, 0, 1))
val empty = List()
```
There are two important **differences between lists and arrays**.
* Lists are immutable — the elements of a list cannot be changed.
* Lists are recursive, while arrays are flat.

## The List Type