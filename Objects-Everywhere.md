# Pure Object Orientation

A pure Object Oriented language is one where every value is an Object. Is Scala a pure Object Oriented language?

Conceptually, types such as `Int` or `Boolean` do not receive special treatment in Scala i.e. they are not primitive types and are just like any other Classes defined in package `scala`. For reasons of efficiency, the Scala compiler represents or maps `scala.Int` by 32 bit integers and `scala.Boolean` by Java's primitive Boolean, etc.

## Primitive Classes as Objects

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

Similarly the `scala.Int` can also be represented using classes.

## Functions as Objects

Functions _are_ treated as Objects by Scala.

`A => B` is basically an abbreviation for `scala.Function1[A,B]` which is roughly defined as:
```scala
trait Function1[A, B] {
    def apply(x: A): B
}
```
So basically functions are `objects` with a `apply()` method.
There are traits for Function2, Function3, ... Function22 for more parameters.

### Expansion of Function Values

An Anonymous function such as
```scala
(x: Int) => x * x
```
is expanded to:
```scala
{ 
   class AnonFun extends Function1[Int, Int] {
      def apply(x: Int): x * x
   }
   new AnonFun
}
```
or, shorter using Anonymous class syntax:
```scala
new Function1[Int, Int] {
   def apply(x: Int): x * x
}
```

A function call, such as `f(a, b)`, where f is a value of some class type, is expanded to
```scala
f.apply(a, b)
```
So the Object Oriented translation of
```scala
val f = (x: Int) => x * x
f(7)
```
would be
```scala
val f = new Function1[Int, Int] {
    def apply(x: Int) = x * x
}
f.apply(7)
```
Note that the apply **method** in this **function** is not an object, else it would expand to a `new Function` containing an apply method and cause an **infinite loop**.

## Functions and Methods
> A **Function Type** is (roughly) a type of the form `(T1, ..., Tn) => U`, which is a shorthand for the `trait FunctionN` in the standard library. Anonymous Functions and Method Values have function types, and function types can be used as part of value, variable and function declarations and definitions. In fact, it can be part of a method type.
>
> A  **Method Type** is a non-value type. That means there is no value - no object, no instance - with a method type. As mentioned above, a Method Value actually has a Function Type. A method type is a def declaration - everything about a def except its body.

Note that a method such as
```scala
def f(x: Int): Boolean = ...
```
is not itself a function value. But if `f` is used in a place where a Function type is expected, it is
converted automatically to the function value
```scala
(x: Int) => f(x)
```
or, expanded:
```scala
new Function1[Int, Boolean] {
    def apply(x: Int) = f(x)
}
```
