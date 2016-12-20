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