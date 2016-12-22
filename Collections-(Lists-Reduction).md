Another common operation is to combine the elements of a list using a given operator: 
* `sum(List(x1, ..., xn))` = 0 + x1 + ... + xn
* `product(List(x1, ..., xn))` = 0 * x1 * ... * xn

We can implement this with the usual recursive schema:
```scala
def sum(xs: List[Int]): Int = xs match {
	case Nil => 0
	case y :: ys => y + sum(ys)
}
```
This can be again generalized using patterns `ReduceLeft`, `foldLeft`, `ReduceRight`

### ReduceLeft (reduce from left)

This pattern can be abstracted out using the generic method `reduceLeft`, which inserts a given binary operator between each successive element of a list.
```scala
List(x1, ..., xn)  reduceLeft op = (...(x1 op x2) op ...) op xn
```
Using `reduceLeft`, we can simplify:

```scala
def sum(xs: List[Int])     = (0 :: xs) reduceLeft ((x,y) => x + y)
def product(xs: List[Int]) = (1 :: xs) reduceLeft ((x,y) => x * y)
```

### Shorter Way to Write Functions

Instead of writing `((x,y) => x * y)`, we can also just write `(_ * _)`.

Every `_` represents a new parameter, going from left to right. The parameters are implicitly defined at the next outer pair of parentheses (or the whole expression if there are no enclosing parentheses)

So:
```scala
def sum(xs: List[Int])     = (0 :: xs) reduceLeft ( _ + _ )
def product(xs: List[Int]) = (1 :: xs) reduceLeft ( _ * _ )
```

### FoldLeft (fold from left)

The function `reduceLeft` is defined in terms of a more general function, `foldLeft`. It's like `reduceLeft`, but it takes an **accumulator**, or zero-element `z`, which is returned when `foldLeft` is called on an empty list.
```scala
(List(x1, ..., xn) foldLeft z)(op) = (...(z op x1) op ...) op xn
```
So,
```scala
def	sum(xs: List[Int]) = (xs foldLeft 0) (_ + _)
def product(xs: List[Int]) = (xs foldLeft 1) (_ * _)
```

### Implementations of ReduceLeft and FoldLeft

```scala
abstract class List[T] { ...
	def reduceLeft(op: (T, T) => T): T = this match {
		case Nil => throw new Error("Nil.reduceLeft")
		case x :: xs => (xs foldLeft x)(op)
	}
	def foldLeft[U](z: U)(op: (U,T) => U): U = this match {
		case Nil => z
		case x :: xs => (xs foldLeft op(z, x))(op)
	}
}
```

### FoldRight, ReduceRight

Applications of `foldLeft` and `reduceLeft` unfold on trees that lean to the left. So trees which lean to right, can use similar `foldRight` and `reduceRight`.

```scala
List(x1, ..., xn)  reduceRight op   = x1 op ( ... (x(n-1) op xn) ... )
(List(x1, ..., xn) foldRight z)(op) = x1 op ( ... (xn op acc) ... )
```

Here is another implementation of Concat
```scala
def concat[T](xs: List[T], ys: List[T]) List[T] = {
    (xs foldRight ys) ( _ :: _ )
}
```
Here, it isn't possible to replace `foldRight` by `foldLeft`. why? The types don't match up. We end up trying to do a `foldLeft` over a list `xs`, meaning we apply an operation over each element of that list - the operation cons is not applicable to arbitrary elements (the accumulators), only lists. Since the order of arguments in `foldRight` is different than `foldLeft` as shown below, we cannot use foldLeft above:
```scala
xs.foldRight(ys){(element, aggregator) => element :: aggregator}
xs.foldLeft(ys){(aggregator, element) => element :: aggregator}
```