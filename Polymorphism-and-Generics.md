# Basics

The example of a `cons-list` i.e. an immutable linked list is used to explain Polymorphism. It has 2 building blocks:
```
Nil    The empty List
Cons   a cell containing an element and the pointer to the remainder of the list
```

Here is a List of Integers in scala using a class hierarchy:
```scala
trait IntList ...
class Cons(val head: Int, val tail: IntList) extends IntList ...
class Nil extends IntList
```
Thus our list will be either:
* empty list i.e `new Nil`
* consist of a head (x) and a tail(xs) i.e `new Cons(x, xs)`

### Value Parameters

Note that in the above definition of Cons class, we have **value parameters** directly in the constructor i.e.
```scala
class Cons(val head: Int, val tail: IntList)
```
is equivalent to 
```
class Cons(_head: Int, _tail: IntList) extends IntList {
val head = _head
val tail = _tail
}
```

### Type Parameters (Generic Parameters)

The List definition can be generalized so that we can define Double Lists, etc:
```scala
trait List[T]
class Cons[T](val head: T, val tail: List[T]) extends List[T]
class Nil[T] extends List[T]
```
Here we are using **Type Parameters** so that we can use any type with this defition: Int, Double, etc.

Thus, the complete definition of List is as below:
```scala
trait List[T] {
    def isEmpty: Boolean
    def head: T
    def tail: List[T]
}

class Cons[T](val head: T, val tail: List[T]) extends List[T] {
    def isEmpty = false
}

class Nil[T] extends List[T] {
    def isEmpty = true
    def head = throw new NoSuchElementException("Nil.head")
    def tail = throw new NoSuchElementException("Nil.tail")
}
```

### Generic Functions

Like classes, functions can also have **Type Parameters**.

Eg. Here is a function that creates a list containing a single element:
```scala
def singleton[T](elem: T) = new Cons[T](elem, new Nil[t])

// usage:
singleton[Int](1)
singleton[Boolean](true)
```
![Generic singleton example usage](https://github.com/rohitvg/scala-principles-1/blob/master/resources/images/type_list_example.png)

### Type Inference

Scala compiler can deduce the type of the value arguments of the function call. So type parameters can be left out.

Hence above example can be called also as:
```scala
// usage:
singleton(1)
singleton(true)
```

### Types and Evaluation

When we use Generics as above, the type information is not stored anywhere. Thus **Type Parameters** do not affect evaluation in Scala and we can assume that all type parameters and type arguments are removed before evaluating a program. This is also called **type erasure**. 

This also happens in Java, Haskell, etc. Languages like C#, C++ keep the type parameters around run time.

# Polymorphism

It means "in many forms". In programming it means:
* A function can be applied to arguments of many types
* A type can have instances of many types

There are 2 typical forms:

1. **Subtyping:** Instances of subclass can be passed to a base class. Eg. List has 2 subtypes Nil, Cons. Here Cons and Nil can be both used as a List.
2. **Generics:** Instances of a function or a class are created by Type Parameterization. Eg. We can create a list of Ints, Doubles, Booleans using type-parameters as above.





