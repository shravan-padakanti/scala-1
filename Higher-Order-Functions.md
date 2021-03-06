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

# Currying

In the above representation of the 3 sum functions using Anonymous functions, we see that `a` and `b` still pass around unchanged. This can be resolved by returning a function from the sum function:

```scala
def sum(f: Int => Int): (Int, Int) => Int = {
    def sumF(a: Int, b: Int): Int =
        if (a > b) 0
        else f(a) + sumF(a + 1, b)
    sumF
}

def sumInts(a: Int, b: Int)       = sum(a => a)
def sumCubes(a: Int, b: Int)      = sum(a => a * a * a)
def sumFactorials(a: Int, b: Int) = sum(a => if (a == 0 || a == 1) 1 else a * fact(a-1))

// This is called using:
sumInts(1,5)
sumCubes(1, 10)
sumFactorials(10, 20)
```

### Multiple Parameter Lists:
In the above format, to avoid middlemenlike sumInts, sumCubes, etc, i.e. for ease of calling them, we can write define the function using **multiple parameter lists** to make it more readable:
```scala
def sum(f: Int => Int)(a: Int, b: Int): Int =
    if (a > b) 0 else f(a) + sum(f)(a + 1, b)

// This can be called using:
sum(cubes)(1, 10)
```
* `sum(cube)` applies sum to cube and returns the _sum of cubes function_.
* `sum(cube)` is therefore equivalent to `sumCubes`
* This function is next applied to the arguments (1, 10).
The function association applies from left to right:
```scala
sum(cube)(1, 10) == (sum (cube)) (1, 10)
```

In general, a definition of a function with multiple parameter lists
```scala
def f(arg1)...(argn) = E
```
where `n > 1`, is equivalent to
```scala
def f(arg1)...(argn−1) = {def g(argn) = E; g}
```
where g is a fresh identifier. Or for short using anonymous function on rhs:
```scala
def f(arg1)...(argsn−1) = (argn => E)
```
... repeating n times ...
```scala
def f = (args1 ⇒ (args2 ⇒ ...(argsn ⇒ E)...))
```

Example:
```scala
def sum(f: Int => Int)(a: Int, b: Int): Int = ...
// what is the 'type' of sum?

// Answer:
// sum is a function which accepts an function which maps from Int to Int i.e (Int => Int) and returns a function which takes in 2 Ints i.e (Int, Int), which in turn returns an Int.
(Int => Int) => (Int, Int) => Int
```
### Another example:
Write a product function which returns the product of the: integers between the given numbers or some function(integers) between the given numbers. Then write a factorial function using that.
```scala
def product(f: Int => Int)(a: Int, b: Int): Int = {
    if (a > b) 1
    else f(a) * product(f)(a+1, b)
}
// call
product(x => x * 3)(3, 4) 

def factorial(a: Int): Int = product(x => x)(1, a)
```
The product and the sum function can be generalized using another function, lets call it mapReduce as below:
```scala
mapReduce(f: Int => Int, combine: (Int, Int) => Int, zero: Int)(a: Int, b: Int): Int = {
    if (a > b) zero // 1 in case of product, 0 in case of sum
    else combine(f(a), mapReduce(a+1, combine, zero)(a+1, b))
}

def product(f: Int => Int)(a: Int, b: Int): Int = mapReduce(f, (a, b) => a * b, 1)(a, b)
```
