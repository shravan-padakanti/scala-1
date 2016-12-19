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

### 1. infix Operator
It is possible to write
```scala
r add s                             r.add(s)
r less s      /* in place of */     r.less(s)
r max s                             r.max(s)
```

### 2. Relaxed Identifiers
Operators hence, can be used as Identifiers (method identifiers) which are either **Alphanumeric** (must start with a letter) or **Symbolic**, ( an _ counts as a letter). <br/>
Eg. `x1`, `*`, `+?%&`, `vector_++`, `counter_=`.

Using these, the above class Rational can be re-written:
```scala
class Rational(x: Int, y: Int) {
    require(y != 0)
    
    def numer = x / g
    def denom = y / g
    
    def + (that: Rational) = ...
    def - (that: Rational) = ...
    ...
}
```
This can be used as below:
```scala
val x = new Rational(1, 2)
val y = new Rational(1, 3)
val p = x + y // instead of x.add(y)
```

Note: To define prefix operators like `-x`, we have to define them as `unary_-`.
Eg. 
```scala
class Rational(x: Int, y: Int) {
    require(y != 0)
    
    def numer = x / g
    def denom = y / g
    
    def unary_-: Rational = new Rational(-numer, denom)
}
// Then we can use:
val x = new Rational(1, 2)
val p = -x // i.e. -1/2
```