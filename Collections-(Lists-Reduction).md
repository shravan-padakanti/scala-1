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

### ReduceLeft

This pattern can be abstracted out using the generic method `reduceLeft`, which inserts a given binary operator between each successive element of a list.

`List(x1, ..., xn)  reduceLeft op = (...(x1 op x2) op ...) op xn`

Using `reduceLeft`, we can simplify:

```scala
def sum(xs: List[Int]) = (0 :: xs) reduceLeft ((x,y) => x + y)
def product(xs: List[Int]) = (1 :: xs) reduceLeft ((x,y) => x * y)
```