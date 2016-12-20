The task we are trying to solve is find a general and convenient way to access objects in a extensible class hierarchy.

Attempts seen previously:
* Classification and access methods: impractical
* Type tests and casts: unsafe, low-level
* Object-oriented decomposition: does not always work, need to touch all classes to add a new method.

## Solution 2: Functional Decomposition with Pattern Matching

**Observation**: the sole purpose of test and accessor functions is to reverse the construction process:
* Which subclass was used?
* What were the arguments of the constructor?
This situation is so common that many functional languages, Scala included, automate it.

### Case Classes:
A case class definition is similar to a normal class definition, except that it is preceded by the modifier case. 
```scala
trait Expr
case class Number(n: Int) extends Expr
case class Sum(e1: Expr, e2: Expr) extends Expr
```
Like before, this defines a trait Expr, and two concrete subclasses `Number` and `Sum`.

It also implicitly defines companion objects with `apply` methods: 
```scala
//IMPLICIT:
object Number {
    def apply(n: Int) = new Number(n)
}

object Sum {
    def apply(e1: Expr, e2: Expr) = new Sum(e1, e2)
}
```
So you can write `Number(1)` instead of `new Number(1)`.
However, these classes are now empty. So how can we access the members?

## Pattern Matching

Pattern matching is a generalization of `switch` from **C/Java** to class hierarchies.
It’s expressed in Scala using the keyword `match`:

```scala
def eval(e: Expr): Int = e match {
    case Number(n) => n
    case Sum(e1, e2) => eval(e1) + eval(e2)
}

// So 
eval(4) // will match case Number
eval(4+3) // will match case Sum
```

Rules:
* match is followed by a sequence of cases, pat => expr.
* Each case associates an expression expr with a pattern pat.
* A MatchError exception is thrown if no pattern matches the value of the selector.


