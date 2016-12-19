# Functions and Data

Here we will learn how functions can create and encapsulate data structures.

# Classes

A class in scala can be defined with its constructor as below:
```scala
class Foo(x: Int, y: Int) {
   def numer = x
   def denom = y
}
```
This definition has 2 entities: 
1. A "type" named Foo
2. A Constructor to create elements of this type.

Scala keeps types and values in different namespaces, so there is no conflict between the definitions of Foo.

# Objects

Objects are elements of a class type, created by using the `new` keyword.
```scala
val foo = new Foo(1,2)
```

# Methods

Functions can be packaged in the class:
```scala
class Foo(x: Int, y: Int) {
   def numer = x
   def denom = y

   def multiplyBy(that: Foo) {
       new Foo( numer * that.numer, denom * that.denom )
   }
}

// call
val foo1 = new Foo(1,2)
val foo2 = new Foo(5,6)
foo1.multiply(foo2)
```

### Some pointers
* **String Representation**: Overwrite the `toString()` method of a class to provide a custom string representation of a class, just like in JAVA.
* **Access Specifiers**: Variables and function definitions in a class that are only for use inside the class can be marked with the **private** access specifier like in JAVA.
* **this**: The `this` keyword can be used inside a class to refer to current object.

### require and assert
The `require` keyword be used to define requirements of a particular class. It takes in a condition and an optional message.

Example: 
```scala
class Foo(x: Int) {
   require( x! = 0, "Integer must be non-zero")

   def numer = x

   def divideBy(that: Foo) {
       new Foo( this.numer / that.numer)
   }
}
```
So if we call:
```scala
val foo1 = new Foo(4)
val foo2 = new Foo(0)
foo1.divideBy(foo2)  // Fails with "IllegalArgumentException: requirement failed: Integer must be non-zero"
```

Similarly `assert` also takes in a condition and an optional message.
```scala
val x = sqrt(y)
assert(x >= 0)

// Fails with AssertionError instead of IllegalArgumentException
```
The difference between `require` and `assert` is the _intent_ of use.
* require is used to enforce a precondition on the caller of a function
* assert is used to check the code of a function

