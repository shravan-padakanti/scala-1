# Pair and Tuples

A pair in scala is written `(x,y)`, where `x` and `y` are the elements of the pair.

```scala
val pair = ("answer", 42)
```

The type of the above pair is `(String, Int)`

We can also decompose pairs using pattern matching, like this:

```scala
val (label, value) = pair // label: String = answer; value: Int = 42
```
This works the same with tuples of **more than two elements**; you can have **triples**, **quadruples**, etc.

### Translation of Tuples

So far, all the types we've encountered are actually abbreviations for some instance of a class type. Tuples are no exception. 

A **tuple type** `(T1, ..., Tn)` is an abbreviation of the parameterized type `scala.Tuple`*n*`[T1, ..., Tn]`

A **tuple expression** of `(e1, ..., en)` is equivalent to the function application `scala.Tuple`*n*`(e1, ..., en)`

Finally, a **tuple pattern** of `(p1, ..., pn)` is equivalent to the constructor pattern `scala.Tuple`*n*`(p1, ..., pn)`