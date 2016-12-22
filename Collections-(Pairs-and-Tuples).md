# Pair and Tuples

A pair in scala is written `(x,y)`, where `x` and `y` are the elements of the pair.
```scala
val pair = ("answer", 42)
```
The type of the above pair is `(String, Int)`

Pairs can be decomposed by pattern matching: 
```scala
val pair = ("answer", 42)
val (label, value) = pair // label: String = answer; value: Int = 42
```
Also, the fields of the pair can be directly accessed by `_1`,`_2`:
```scala
val pair = ("answer", 42)
label = pair._1
value = pair._2
```

This works the same with tuples of **more than two elements**; you can have **triples**, **quadruples**, etc.

### Translation of Tuples

So far, all the types we've encountered are actually abbreviations for some instance of a class type. Tuples are no exception. 

* A **tuple type** `(T1, ..., Tn)` is an abbreviation of the parameterized type `scala.Tuple`*n*`[T1, ..., Tn]`
* A **tuple expression** of `(e1, ..., en)` is equivalent to the function application `scala.Tuple`*n*`(e1, ..., en)`
* A **tuple pattern** of `(p1, ..., pn)` is equivalent to the constructor pattern `scala.Tuple`*n*`(p1, ..., pn)`

### Merge sort using Pairs

[Insertion sort](https://github.com/rohitvg/scala-principles-1/wiki/Collections-(Lists)#sorting-lists) was slow (quadratic).
Previous implementation of `merge` in [Merge Sort](https://github.com/rohitvg/scala-principles-1/wiki/Collections-(List-Methods)#sorting-lists-faster) had issues as mentioned. 

Here is a better implementation using pattern matching over pairs:
```scala
def msort(xs List[Int]): List[Int] = {
  val n = xs.length / 2
  if (n == 0) xs
  else {
    def merge(xs: List[Int], ys: List[Int]): List[Int] = (xs, ys) match {
      case (Nil, ys) => ys
      case (xs, Nil) => xs
      case (x :: xs1, y :: ys1) =>
        if (x < y) x :: merge(xs1, ys)
        else y :: merge(xs, ys1)
    }
    val (fst, snd) = xs splitAt n
    merge(msort(fst), msort(snd))
  }
}
```