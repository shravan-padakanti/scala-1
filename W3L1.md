# Class Hierarchies
> **Static vs Dynamic Binding**
>
> In the most general terms, static binding means that references are resolved at compile time.
> ```java
> Animal a = new Animal();
> a.Roar(); // The compiler can resolve this method call statically.
> ```
> Dynamic binding means that references are resolved at run time.
> ```java
> public void MakeSomeNoise(object a) {
>  // Things happen...
>  ((Animal) a).Roar(); // You won't know if this works until runtime!
> }
> '''

For class hierarchies we will be taking the example of IntegerSets.

### Abstract Classes
These are the classes which are missing an implementation i.e just have the method signature. Consequently these classes cannot be instantiated using `new`.
```scala
abstract class IntSet{
    def incl(x: Int): IntSet
    def contains(x: Int): Boolean
}
```

### Class Extension
Like in Java, classes and abstract classes can be extended by using the `extends` keyword.

Here we will implement the set as a **Binary Tree**. We maintain the _invariant_ that the nodes on the right hand side of any node have higher values than the nodes on the left hand side of that node. This will allow us to implement the contains method easily.
> A binary tree is made of nodes, where each node contains a "left" pointer, a "right" pointer, and a data element. The "root" pointer points to the topmost node in the tree.

An "empty" set can be implemented as below:
```scala
class Empty extends IntSet {
    def incl(x: Int): IntSet = new NonEmpty(x, new Empty, new Empty)
    def contains(x: Int): Boolean = false
    override def toString() = "."
}
```
A "non-empty" set can be implemented as below:
```scala
class NonEmpty(elem: Int, left: IntSet, right: IntSet) extends IntSet {
    def contains(x: Int): Boolean =
        if (x < elem) left contains x
        else if (x > elem) right contains x
        else true

    def incl(x: Int): IntSet =
        if (x < elem) new NonEmpty(elem, left incl x, right)
        else if (x > elem) new NonEmpty(elem, left, right incl x)
        else this

    override def toString() = "{" + left + elem + right + "}"
}
```
In this particular implementation of NonEmpty set, whenever `incl()` is called to include a new value, a new branch of tree is created. These type of data structure are called as **Persistent Data Structures**, as even when changes are made, the old version of the data structure stays.

Thus here, the Empty and NonEmpty classes `extend` the class IntSet. This implies that the types Empty and NonEmpty conform to the type IntSet: an object of type Empty or NonEmpty can be used wherever an
object of type IntSet is required.

* IntSet is called the **superclass** of Empty and NonEmpty.
* Empty and NonEmpty are **subclasses** of IntSet.
* In Scala, any user-defined class extends another class. If no superclass is given, the standard class Object in the Java package `java.lang` is assumed.
* The direct or indirect superclasses of a class C are called **base classes** of C. So, the base classes of NonEmpty are IntSet and Object.

### Implementation and Overriding

The definitions of contains and incl in the classes Empty and NonEmpty implement the abstract functions in the base class IntSet.

It is also possible to redefine an existing, non-abstract definition in a subclass by using **override**. Eg.
```scala
abstract class Base {                                 class Sub extends Base {
    def foo = 1                                           override def foo = 2
    def bar: Int                                          def bar = 3
}                                                     }
```

### Object Definitions
**Objects** are **singleton objects** i.e. single instance. No other objects can be created. These can be used directly and don't need the `new` keyword for instantiation. 

In the IntSet example, one could argue that there is really only a single empty IntSet. So it seems overkill to have the user create many instances of it. We can express this case better with an **object definition**.

```scala
object Empty extends IntSet {
    def contains(x: Int): Boolean = false
    def incl(x: Int): IntSet = new NonEmpty(x, Empty, Empty)
}
```

### Programs
It is possible to create standalone applications in Scala. Each such application contains an **object** with a `main` method. Eg:
```scala
object Hello {
    def main(args: Array[String]) = println(”hello world!”)
}
```
Once this program is compiled, you can start it from the command line with:
```
> scala Hello
```