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