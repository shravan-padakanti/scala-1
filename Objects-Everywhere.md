### Pure Object Orientation

A pure Object Oriented language is one where every value is an Object. Is Scala a pure Object Oriented language?

Conceptually, types such as `Int` or `Boolean` do not receive special treatment in Scala i.e. they are not primitive types and are just like any other Classes defined in package `scala`. For reasons of efficiency, the Scala compiler represents or maps `scala.Int` by 32 bit integers and `scala.Boolean` by Java's primitive Boolean, etc.

### Pure Booleans

Scala compiler represents or maps `scala.Boolean` by Java's primitive Boolean, but it can be defined as a Class from the first principles without resorting to primitive Booleans:
```scala
package idealized.scala

abstract class Boolean extends AnyVal {
    // arg t is the expression for the 'if then' part and 
    // arg e is the expression for the 'else' part.
    // so basically instead of if (condition) x else y, with this
    // class we will use condition.ifThenElse(x, y)
    // We will define the other operators using this 
    // method.
    def ifThenElse[T](t: => T, e: => T): T

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