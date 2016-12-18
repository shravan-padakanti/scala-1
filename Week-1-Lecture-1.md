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
| **Inf. Loop**      |               |

