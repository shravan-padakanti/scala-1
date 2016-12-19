Consider the class:
```scala
class Rational(x: Int, y: Int) {
  require(y != 0)

  def numer = x
  def numer = y

  def add(that: Rational) = ...
  def subtract(that: Rational) = ...
}
```

In principle, the rational numbers defined by Rational are as natural as integers. But for the user of these abstractions, there is a noticeable difference:
* We write `x + y`, if x and y are **integers**, but
* We write `r.add(s)` if r and s are **Rational** instances.
In Scala, we can eliminate this difference. We proceed in two steps.

# 1. infix Operator
It is possible to write
```scala
r add s                             r.add(s)
r less s      /* in place of */     r.less(s)
r max s                             r.max(s)
```

# 2. Relaxed Identifiers
Operators hence, can be used as Identifiers (method identifiers) which are either **Alphanumeric** (must start with a letter) or **Symbolic**, ( an _ counts as a letter). <br/>
Eg. `x1`, `*`, `+?%&`, `vector_++`, `counter_=`
