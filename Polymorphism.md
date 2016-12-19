# Polymorphism

The example of a `cons-list` i.e. an immutable linked list is used to explain Polymorphism. It has 2 building blocks:
```
Nil    The empty List
Cons   a cell containing an element and the pointer to the remainder of the list
```

Here is a List of Integers in scala using a class hierarchy:
```scala
trait IntList ...
class Cons(val head: Int, val tail: IntList) extends IntList ...
class Nil extends IntList // So that an IntList can be Nil
```
Thus a list is either:
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

### Type Parameters

The List definition can be generalized so that we can define Double Lists, etc:
```scala
trait List[T]
class Cons[T](val head: T, val tail: List[T]) extends List[T]
class Nil[T] extends List[T]
```
Here we are using **Type Parameters** so that we can use any type with this defition: Int, Double, etc.

The complete definition of List is as below:
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



