# Programming Paradigms

### Imperative programming
An imperative language uses a sequence of statements to determine how to reach a certain goal. (Eg. C, C++, Java). Eg. Below, each statement changes the state of the program, from assigning values to each variable to the final addition of those values.
```java
int total = 0;
int number1 = 5;
int number2 = 10;
total = number1 + number2; 
```
As seen imperative programming involves modifying mutable variables, using assignments, control structures like if-else, for, break, continue, etc.

**Problem:**<br/>
The imperative programming is what programmers use mostly in their day to day life. The problem is **scaling**. Imperative programming *conceptualizes programs word by word*. To solve this, we need higher level abstractions of collections, polynomials, geometric shapes, etc. Hence we come to **Functional Programming**.

### Functional programming: 
A functional language takes functional approach to problem solving i.e. based on functions. Functions are first class citizens i.e. whatever you can do with data i.e. declare it anywhere, pass it as parameters, use operators on them, etc, you can do that on Functions and thus use functions to form the program. Eg. List, Python, Scala. Note that Lisp is a functional programming language in a restricted sense. Python and Scala are functional programming languages in a _wider sense_ meaning they also count as object oriented languages.
Functional programming is becoming popular due to:
* simpler reasoning principles
* better modularity
* good for **exploiting parallelism** for multicore and cloud computing. Watch https://www.youtube.com/watch?v=3jg1AheF4n0 for information on this by Martin Odersky.

### Logic programming: 
Any program written in a logic programming language is a set of sentences in logical form, expressing facts and rules about some problem domain. The rules are written in the form of clauses (Eg. Prolog, Datalog).

### Object oriented programming: 
A OOP language takes an object based approach to problem solving. Objects are first class citizens i.e. whatever you can do with data i.e. declare it anywhere, pass it as parameters, use operators on them, etc, you can do that on Objects and thus use objects to form the program. (Eg. Java)

# Java vs Scala
Doing concurrent programming in Java requires writing and managing threads, async blocks, locks and releasing locks, etc. Scala makes this simple by providing parallel collections and lambda expressions.
> Note: Java8 tries to improve on this problem in Java by introducing lambda expressions.

# Elements of Programming in Scala

> `b + 1` is and expression, while `a = b + 1`; is a statement. A statement forms a complete unit of execution.

Functional programming is like a calculator. An interactive shell lets one write expressions and responds with their value (REPL: Read-Eval-Print-Loop).

Scala uses `def` to define variables. The definitions can have parameters:
```scala
def example = 2      // evaluated when called
val example = 2      // evaluated immediately
lazy val example = 2 // evaluated once when needed

def square(x: Double) = x * x
# return type can be specified:
def square(x: Double): Double = x * x
```
**Note:** Specifying return type for functions is optional, but if the function is recursive then its mandatory.
Primitive datatypes are same as in Java but capitalized: `Int, Double, Boolean`

### Substitution Model: 
An expression is evaluated i.e. is reduced to a value using substitution. Evaluation of expression takes place from left to right.
```Scala
sumOfSquares(3, 2+2)
sumOfSquares(3, 4)
square(3) + square(4)
3 * 3 + square(4)
9 + square(4)
9 + 4 * 4
25
```
This model can be applied to all the expressions as long as there is no _side effect_. Eg. The expression `x++` where variable x returns old value of x and also increment the value of x so that the next use uses the updated value. The model works by using either the **call-by-value** or **call-by-name** evaluation strategy, based on which is fastest. Both strategies yield the same result **provided the evaluations terminate**.

**Call-by-value** has the advantage that it evaluates every function argument only once. Also it computes the passed-in expression's value before calling the function, thus the same value is accessed every time.

**Call-by-name** has the advantage that a function argument is not evaluated if the corresponding parameter is unused in the evaluation of the function body. Also it recomputes the passed-in expression's value every time it is accessed.


Example:
```Scala

// Function with a side-effect
def getx():Int = {
  println("Hello world")
  1 // return value
}

def callByValue(x: Int) = {
  println("x1=" + x)
  println("x2=" + x)
}

def callByName(x: => Int) = {
  println("x1=" + x)
  println("x2=" + x)
}

// Call the above functions and the output
scala> callByValue(getx())
Hello world
x1=1
x2=1

scala> callByName(getx())
Hello world
x1=1
Hello world
x2=1

```

### More examples CBV vs CBN
Consider the function:
```Scala
def test(x: Int, y: Int) = x * x
```

1) `test(2,3)`

| CBV           | CBN           |
|:-------------:|:-------------:|
| 2 * 2         | 2 * 2         |
| 4             | 4             |
| **SAME**      | **SAME**      |


2) `test(3+4,8)`

| CBV           | CBN           |
|:-------------:|:-------------:|
| test(7,8)     | (3+4) * (3+4) |
| 7 * 7         | (3+4) * (3+4) |
| 49            | 7 * 7         |
| **FASTER**    | 49            |

3) `test(7, 2*4)`

| CBV           | CBN           |
|:-------------:|:-------------:|
| test(7,8)     | 7 * 7         |
| 7 * 7         | 49            |
| 49            | **FASTER**    |

4) `test(3+4, 2*4)`

| CBV           | CBN           |
|:-------------:|:-------------:|
| test(7,8)     | (3+4) * (3+4) |
| 7 * 7         | 7 * 7         |
| 49            | 49            |
| **SAME**      | **SAME**      |

### What if the expression does not terminate?
If CBV terminates, the CBN terminates as well. Vice versa is not true.

Eg. Consider the function: 
```scala
def first(x: Int, y: Int) = x
```
For the call: `first(1, loop)`

| CBV           | CBN           |
|:-------------:|:-------------:|
| first(1, loop)| 1             |
| first(1, loop)| **ENDED**     |
| first(1, loop)|               |
| ...           |               |
| **Inf. Loop** |               |


Turns out that for practical purposes CBV is more efficient than CBN and hence **Scala uses CBV by default**. But **CBN can be forced** by using `=>`.
Eg. 
```scala
def callByValue(x: Int) = {
  println("x1=" + x)
  println("x2=" + x)
}

def callByName(x: => Int) = {
  println("x1=" + x)
  println("x2=" + x)
}
```

### Implications:

Eg. Consider the function: 
```scala
def constOne(x: Int, y:=> Int) = x
```

| `constOne(1+2, loop)` | `constOne(loop, 1+2)` |
|:-------------:|:-------------:|
| first(3, loop)| first(loop, 1+2)|
| 1             | first(loop, 1+2)|
| **ENDED**     | ...  |
|               | **Inf. loop**  |

# Conditional Expressions

### if - else
It looks like if - else for Java but is used for expressions and not statements.
```scala
def abs(x: Int) = if (x >= 0) x else -x
```
Sometimes `&&` or `||` do not require the right-hand operand to be evaluated. This is called as **short-circuit evaluation**
```scala
true || e  // is true irrespective of e
false || e // is false irrespective of e
```

### Value Definitions
`def` always uses **by-name**.
To use **by-value**, we use `val`.
```scala
val x = 2
def y = square(x) // y is square(2). Console prints Int after this is entered. print(y) prints 4.
val y = square(x) // y is 4 directly and not square(2). Console would directly print y = 4 after this is entered.

def loop: Boolean = loop  // a method that does nothing but calls itself recursively
def x = loop // OK, yields x: Boolean on the console
val x = loop // Infinite loop as the 
```

# Blocks and Lexical Scope

Scala allows **Nested Functions** so that the functions that are not supposed to be accessed by the user can be nested inside the ones that are accessible.

A block is defined using curly braces: ` { .... }`. The last element of a block is an expression that defines its value i.e. return expression. Blocks are themselves expressions; a block may appear everywhere an expression can.

The definitions inside a block are only visible from inside that block. The definitions inside a block shadow definitions of the same names outside the block.

```scala
val x = 0
def f(y: Int) = y + 1
val result = {
  val x = f(3); // f is not shadowed inside the block, x is.
  x * x
} + x

// Here result = 16
```

** Semicolons ** are optional, used to separate multiple expressions written on the same line.

Expressions extending to multiple lines can be written by using curly braces:
```
( expression1 
+ expression2 )
```
Or, the operator can be written as the end of first line indicating that the operation is not yet finished.
```
expression1 + 
expression2
```

# Recursion

If a function calls itself as the last action, the functions stack frame can be reused. This is called **Tail recursion**. 
Eg. `gcd` is tail-recursive whereas `factorial` is not:
```
// gcd -> Last step is pure gcd.
gcd(14, 21)
→ if (21 == 0) 14 else gcd(21, 14 % 21)
→ if (false) 14 else gcd(21, 14 % 21)
→ gcd(21, 14 % 21)
→ gcd(21, 14)
→ if (14 == 0) 21 else gcd(14, 21 % 14)
→→ gcd(14, 7)
→→ gcd(7, 0)
→ if (0 == 0) 7 else gcd(0, 7 % 0)
→ 7

// factorial -> Last step factorial() multiplied by something
→ if (4 == 0) 1 else 4 * factorial(4 - 1)
→→ 4 * factorial(3)
→→ 4 * (3 * factorial(2))
→→ 4 * (3 * (2 * factorial(1)))
→→ 4 * (3 * (2 * (1 * factorial(0)))
→→ 4 * (3 * (2 * (1 * 1)))
→→ 120
```
**The Scala compiler will automatically optimize any truly tail-recursive method**. If you annotate a method that you believe is tail-recursive with the `@tailrec` annotation, then the compiler will warn you if the method is actually not tail-recursive. This makes the `@tailrec` annotation a good idea, both to ensure that a method is currently optimizable and that it remains optimizable as it is modified.

Note that Scala does not consider a method to be tail-recursive if it can be overridden. Thus the method must either be private, final, on an object (as opposed to a class or trait), or inside another method to be optimized.
```scala
  def factorial(x: Int): Int = {
    if (x == 0) 1 else (x * factorial(x - 1))
  }
  // calculates factorial of 999 in 239337ns

  def tailRecursiveFactorial(x: Int): Int = {
    @tailrec
    def loop(acc: Int, x: Int): Int = if (x == 0) acc else loop(acc * x, (x - 1))
    loop(1, x)
  // calculates factorial of 999 in 49573ns
  }
```