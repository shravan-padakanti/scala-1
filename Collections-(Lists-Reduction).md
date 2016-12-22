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