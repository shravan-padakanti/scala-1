Here we will see how to handle nested-sequences.

The calculations which are usually expressed in loops can be expressed using higher order functions in sequences.<br/>
Eg. Given a positive integer n, find all pairs of positive integers i and j, ith 1 <= j < i < n such that (i+j) is prime. So if n = 7, we would get the below pairs:
```
i     |  2  3  4  4  5  6  6
j     |  1  2  1  3  2  1  5
------------------------------
i + j |  3  5  5  7  7  7  11
```
In a conventional way, we would use 2 for loops which are nested, 1 for i and 1 for j. How would you do this in functional programming?

### Algorithm:

A natural way to do this is to:
* Generate the sequence of all pairs of integers `(i, j)` such that `1 <= j < i < n`.
* Filter the pairs for which `i + j` is prime.

One natural way to generate the sequence of pairs is to:
* Generate all the integers `i` between `1 and n (excluded)`.
* For each integer `i`, generate the list of pairs `(i, 1), ..., (i,i-1)`.

This can be achieved by combining until and map:
```scala
(1 until n) map (i => (1 until i) map (j => (i, j)))
```
This generates a vector of vectors. 
``` 
Vector( Vector(), Vector((2,1)), Vector((3,1), (3,2)) ... , Vector((6,1), (6,2), (6,3), (6,4), (6,5)) )
```
This is because `range` is a subtype of `seq`. We started with a range (1 until n) and transformed it using a map, which produced a seq. Pairs cannot be elements of range, so what the type inference chose `IndexedSequence`, essentially a sequence that uses random access and sits between `Seq` and `Range`. The prototypical default implementation of an IndexedSequence is just a Vector.
![https://github.com/rohitvg/scala-principles-1/commit/4e6f441b9ebf6a23351b3a5e6661465c68711bac](Indexed Sequence)

This is not right as we generate a collection of pairs. So we want to concatenate the above list. Hence:
```scala
((1 until n) map (i => (1 until i) map (j => (i, j))) foldRight Seq[Int]())( _ ++ _ )
// OR
(1 until n) map (i => (1 until i) map (j => (i, j))).flatten // inbuilt method to flatten
```

We know that the `flatMap` function works in a similar fashion, i.e.
```scala
s flatMap f = (xs map f).flatten
```
so we can simplify the above expression to:
```scala
(1 until n) flatMap (i => (1 until i) map (j => (i, j)))
```
Now that we have the desired pair sequence, we need to filter our sequence according to the criterion, that the sum of the pair is prime:
```scala
def isPrime(n: Int) = (2 until n) forall (n % _ != 0)
(1 until n) flatMap  (i => (1 until i) map (j => (i, j))) filter (pair => isPrime(pair._1 + pair._2))
```

**Problem**: is there a simpler way to organize this expression that makes it more understandable?

## For-Expressions

Higher-order functions such as `map`, `flatMap` or filter provide powerful constructs for manipulating lists.
But sometimes the level of abstraction required by these function make the program difficult to understand.
In this case, Scala’s `for expression` notation can help.

Let persons be a list of elements of class Person, with fields name and age.
```scala
case class Person(name: String, age: Int)
```
To obtain the names of persons over 20 years old, you can write:
```scala
for ( p <- persons if p.age > 20 ) yield p.name
```
which is equivalent to:
```scala
persons filter (p => p.age > 20) map (p => p.name)
```
**The for-expression is similar to loops in imperative languages, except that it builds a list of the results of all iterations**.

### Syntax

A for-expression is of the form
```scala
for ( s ) yield e
```
where `s` is a _sequence_ of **generators** and **filters**, and `e` is an expression whose value is returned by an iteration.
* A **generator** is of the form `p <- e`, where `p` is a pattern and `e` an expression whose value is a collection.
* A **filter** is of the form `if f` where `f` is a boolean expression.
* The sequence must start with a generator.
* If there are several generators in the sequence, the last generators vary faster than the first.

Instead of `( s )`, braces `{ s }` can also be used, and then the sequence of generators and filters can be written on multiple lines without requiring semicolons.

### Example:
The above example can be implemented as:
```scala
for {
    i <- 1 until n        // sequence
    j <- 1 until i        // sequence
    if isPrime(i + j)     // filter
} yield (i, j)
```