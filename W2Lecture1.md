# Higher Order Functions

Functions that take other functions as parameters, or functions that return other functions are called **Higher order functions**. This is unlike **First order functions** which act on or return datatypes, like Int, String, Lists, etc but not other functions.

### Function Type
The type `X => Y` is a type that takes an argument of type X and returns result of type Y i.e. maps X to Y.

### Example
Consider 3 functions which takes the integers between 2 given numbers, returns their sum, sum of cubes, sum of factorials:

```scala
def sumInts(a: Int, b: Int): Int       = if (a > b) 0 else a + sumInts(a + 1, b)
def sumCubes(a: Int, b: Int): Int      = if (a > b) 0 else cube(a) + sumCubes(a + 1, b)
def sumFactorials(a: Int, b: Int): Int = if (a > b) 0 else fact(a) + sumFactorials(a + 1, b)

// where
def cube(a: Int): Int = a * a * a
def fact(a: Int): Int = if (a == 0 || a == 1) 1 else a * fact(a-1)
```
This can be written using a **higher-order-function** as below
```scala
def sum(f: Int => Int, a: Int, b: Int): Int = {
    if (a > b) 0
    else f(a) + sum(f, a + 1, b)
}

def sumInts(a: Int, b: Int)       = sum(id, a, b)
def sumCubes(a: Int, b: Int)      = sum(cube, a, b)
def sumFactorials(a: Int, b: Int) = sum(fact, a, b)

// where
def id(a: Int): Int   = a
def cube(a: Int): Int = a * a * a
def fact(a: Int): Int = if (a == 0 || a == 1) 1 else a * fact(a-1)
```

### Annonymous Functions

As seen above, passing functions as parameters leads to creation of many small functions which can become tedious.

Note that Strings are **literals**. Strings don't need to be defined using `def str = "Hello!"; println(str)`. Instead we can directly use them as literals `println("Hello!")`. We want to be able to do the same to functions and hence **Anonymous Functions**. These can be done by writing 
```scala
def foo(x: Int, y: Int) = { body }
``` 
as
```scala
(x: Int, y: Int) => { body }
```
An anonymous function `(x1 : T1, ..., xn : Tn) => E` can always be expressed using def as `def f(x1 : T1, ..., xn : Tn) = E; f`

**Note**: Anonymous functions are syntactic sugar, do not add anything else to the programming language.

So using the Anonymous functions, the above example can be written as:

```scala
def sum(f: Int => Int, a: Int, b: Int): Int = {
    if (a > b) 0
    else f(a) + sum(f, a + 1, b)
}

def sumInts(a: Int, b: Int)       = sum(a => a, a, b)
def sumCubes(a: Int, b: Int)      = sum(a => a * a * a, a, b)
def sumFactorials(a: Int, b: Int) = sum(a => if (a == 0 || a == 1) 1 else a * fact(a-1), a, b)
```