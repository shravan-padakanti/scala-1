### Pure Object Orientation

A pure Object Oriented language is one where every value is an Object. Is Scala a pure Object Oriented language?

Conceptually, types such as `Int` or `Boolean` do not receive special treatment in Scala i.e. they are not primitive types and are just like any other Classes defined in package `scala`. For reasons of efficiency, the Scala compiler represents or maps `scala.Int` by 32 bit integers and `scala.Boolean` by Java's primitive Boolean, etc.

### Pure Booleans

Scala compiler represents or maps `scala.Boolean` by Java's primitive Boolean, but it can be defined as a Class from the first principles without resorting to primitive Booleans:
```scala
package idealized.scala

abstract class Boolean extends AnyVal {
    def ifThenElse[T](t: => T, e: => T): T

    // The false and true here are also from the idealized.scala package
    def && (x: => Boolean): Boolean = ifThenElse(x, false)
    def || (x: => Boolean): Boolean = ifThenElse(true, x)
    def unary_!: Boolean            = ifThenElse(false, true)

    def == (x: Boolean): Boolean    = ifThenElse(x, x.unary_!)
    def != (x: Boolean): Boolean    = ifThenElse(x.unary_!, x)
...
}

object true extends Boolean {
    def ifThenElse[T](t: => T, e: => T) = t
}

object false extends Boolean {
    def ifThenElse[T](t: => T, e: => T) = e
}
```
So as seen above, in the method `ifThenElse`, arg `t` is the expression for the 'if then' part and arg `e` is the expression for the 'else' part. So basically instead of 
```scala
let condition : idealized.scala.Boolean = ...
if (condition) x else y
```
since we are using our own implementation of boolean, the condition will evaluate using `idealized.scala.Boolean` instead of the `scala.Boolean`. Hence we will use it as:
```scala
condition.ifThenElse(x, y)
```
Similarly, we will use the other operators as:
```scala
let condition1 : idealized.scala.Boolean = ...
let condition2 : idealized.scala.Boolean = ...

condition1 && condition 2
condition1 == condition 2 
etc
```
