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

### List Constructor

All lists are constructed from:
* the empty list `Nil`, and
* the construction operation `::` (pronounced cons)

`x :: xs` gives a new list with the first element x, followed by the elements of xs.
For example:
```scala
val fruit = ”apples” :: (”oranges” :: (”pears” :: Nil))
val nums  = 1 :: (2 :: (3 :: (4 :: Nil)))
val empty = Nil
```

### Right Associativity

Convention: Operators ending in `:` associate to the right, thus `A :: B :: C` is interpreted as `A :: (B :: C).`

So we can thus omit the parentheses in the definition above.
```scala
val nums = 1 :: 2 :: 3 :: 4 :: Nil
```
Operators ending in `:` are also different in the they are seen as method calls of the right-hand operand. (`::` is analogus to a prepend operation)

So the expression above is equivalent to
```
Nil.::(4).::(3).::(2).::(1)
```

### List Operations
All operations on lists can be expressed in terms of the following three operations:
1. **head** the first element of the list
2. **tail** the list composed of all the elements except the first.
3. **isEmpty** ‘true‘ if the list is empty, ‘false‘ otherwise.
These operations are defined as methods of objects of type list. For example:
```scala
fruit.head == ”apples”
fruit.tail.head == ”oranges”
diag3.head == List(1, 0, 0)
empty.head == throw new NoSuchElementException(”head of empty list”)
```

### List Patterns
It is also possible to decompose lists with pattern matching. Patterns:
```
Nil                     The Nil constant
p :: ps                 A pattern that matches a list with a head matching p and a tail matching ps.
List(p1, ..., pn)       same as p1 :: ... :: pn :: Nil
```
Example
```
1 :: 2 :: xs     Lists of that start with 1 and then 2
x :: Nil         Lists of length 1
List(x)          Same as x :: Nil
List()           The empty list, same as Nil
List(2 :: xs)    A list that contains as only element another list that starts with 2.
```

### Sorting Lists
