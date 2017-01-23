> ### Brief explanation:
> 
>
> * **Call-by-value:** The expression is evaluated once when it is declared irresepctive of if its referenced or not. If it is then that evaluated "value" is used thereafter whenever that expression is referenced.
> * **Lazy-evaluation:** The expression is evaluated only when its referenced for the first time. For all later times that x is referenced, the same value is used.
> * **Call-by-name:** The expression is evaluated every time it is referenced.
>
> Some reading on CBN, CBV: http://stackoverflow.com/questions/13337338/call-by-name-vs-call-by-value-in-scala-clarification-needed

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

### Definitions and Value Definitions

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
**The Scala compiler will automatically optimize any truly tail-recursive method**. If you annotate a method that you believe is tail-recursive with the `@scala.annotation.tailrec` annotation, then the compiler will warn you if the method is actually not tail-recursive. This makes the `@tailrec` annotation a good idea, both to ensure that a method is currently optimizable and that it tells you with an error in case it is modified later on.

Note that Scala does not consider a method to be tail-recursive if it can be overridden. Thus the method **must either be private, final, on an object** (as opposed to a class or trait), **or inside another method** to be optimized.
```scala
  def factorial(x: Int): Int = {
    if (x == 0) 1 
    else (x * factorial(x - 1))
  }
  // calculates factorial of 999 in 239337ns

  def tailRecursiveFactorial(x: Int): Int = {
    @tailrec
    def loop(acc: Int, x: Int): Int = { 
        if (x == 0) acc 
        else loop(acc * x, (x - 1))
    }
    loop(1, x)
  }
  // calculates factorial of 999 in 49573ns
```
