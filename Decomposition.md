Suppose you want to write a small interpreter for arithmetic expressions.

To keep it simple, let’s restrict ourselves to numbers and additions with the following implementations:
```scala
trait Expr {
    def isNumber: Boolean        // for classification if its a num or sum
    def isSum: Boolean           // for classification if its a num or sum
    def numValue: Int            // for access
    def leftOp: Expr             // for access
    def rightOp: Expr            // for access
}

class Number(n: Int) extends Expr {
    def isNumber: Boolean = true
    def isSum: Boolean = false
    def numValue: Int = n
    def leftOp: Expr = throw new Error("I am a Number, no leftOp")
    def rightOp: Expr = throw new Error("I am a Number, no rightOp")
}

class Sum(e1: Expr, e2: Expr) extends Expr {
    def isNumber: Boolean = false
    def isSum: Boolean = true
    def numValue: Int = throw new Error(”I am a sum expression, not a number”)
    def leftOp: Expr = e1 
    def rightOp: Expr = e2
}
```
An evaluation function can be written as follows:
```scala
def eval(e: Expr): Int = {
    if (e.isNumber) e.numValue
    else if (e.isSum) eval(e.leftOp) + eval(e.rightOp)
    else throw new Error(”Unknown expression ” + e)
}
```
**Problem**: Writing all these classification and accessor functions quickly becomes tedious! So, what happens if you want to add new expression forms, say
```scala
class Prod(e1: Expr, e2: Expr) extends Expr // e1 * e2
class Var(x: String) extends Expr           // Variable ‘x’
```
You need to add methods for classification and access to all classes defined above i.e. we will need `isProd`, access to product expressions, `isVar`, access to variable value, etc. Just for sum and number we have defined 10 methods.

So how to address this issue?

### Non-Solution: Type Tests and Type Casts

A "hacky" solution could use type tests and type casts. Scala let’s you do these using methods defined in class `Any`:
```scala
def isInstanceOf[T]: Boolean // checks whether this object’s type conforms to ‘T‘
def asInstanceOf[T]: T       // treats this object as an instance of type ‘T‘
                             // throws ‘ClassCastException‘ if it isn’t.

// So implente the eval as:
def eval(e: Expr): Int = {
    if (e.isInstanceOf[Number])
        e.asInstanceOf[Number].numValue
    else if (e.isInstanceOf[Sum])
        eval(e.asInstanceOf[Sum].leftOp) +
        eval(e.asInstanceOf[Sum].rightOp)
    else throw new Error(”Unknown expression ” + e)
}
```
Here:

* **Pros:** no need for classification methods, access methods only for classes where the value is defined.
* **Cons:** low-level and potentially unsafe.
