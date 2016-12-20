The list is a fundamental data structure in functional programming. A list having `x1, ..., xn` as elements is written `List(x1, ..., xn)`.

Example
```scala
val fruit = List(”apples”, ”oranges”, ”pears”)
val nums  = List(1, 2, 3, 4)
val diag3 = List(List(1, 0, 0), List(0, 1, 0), List(0, 0, 1))
val empty = List()
```
![scala lists visualization](scala_lists_visualization)

Like arrays, lists are homogeneous: the elements of a list must all have the same type.
The type of a list with elements of type T is written `scala.List[T]` or shorter `just List[T]`
Example
```scala
val fruit: List[String]    = List(”apples”, ”oranges”, ”pears”)
val nums : List[Int]       = List(1, 2, 3, 4)
val diag3: List[List[Int]] = List(List(1, 0, 0), List(0, 1, 0), List(0, 0, 1))
val empty: List[Nothing]   = List()
```
There are two important **differences between lists and arrays**.
* Lists are immutable — the elements of a list cannot be changed.
* Lists are recursive, while arrays are flat.

## List Constructor

All lists are constructed from:
* the empty list `Nil`, and
* the construction operation `::` (pronounced cons)

`x :: xs` gives a new list with the first element x, followed by the elements of xs.
For example:
```scala
fruit = ”apples” :: (”oranges” :: (”pears” :: Nil))
nums = 1 :: (2 :: (3 :: (4 :: Nil)))
empty = Nil
```

