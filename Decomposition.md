Suppose you want to write a small interpreter for arithmetic expressions.

To keep it simple, let’s restrict ourselves to numbers and additions with the following implementations:
```scala
trait Expr {
    def isNumber: Boolean
    def isSum: Boolean
    def numValue: Int
    def leftOp: Expr
    def rightOp: Expr
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
```
class Prod(e1: Expr, e2: Expr) extends Expr // e1 * e2
class Var(x: String) extends Expr           // Variable ‘x’
```
You need to add methods for classification and access to all classes defined above.

