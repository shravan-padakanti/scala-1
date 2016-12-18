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


