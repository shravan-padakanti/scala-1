# Organizing Classes 

### Packages

Like Java, classes are organized using **Packages**. To place a class or object inside a package, the `package` clause is placed at the top the source file.
Eg. To place an object called `Hello` in the package `progfun.examples`
```scala
package progfun.examples

object Hello { 
    ... 
}
```
A class or an object can be referred by its fully qualified name i.e. **packagename.classname** Eg. In the above case, `progfun.examples.Hello`. So to run the program, we can use:
```
> scala progfun.examples.Hello
```

### Imports

As mentioned above, a class can be used using its fully qualified name:
```scala
val num1 = new progfun.examples.Rational(1,2)
```
Alternatively, you can use an `import`, and just use the class name or object name:
```scala
import week3.Rational
val foo = new Rational(1, 2)
```
imports can be used in various ways:
```scala
// Named imports
import week3.Rational          // imports just Rational
import week3.{Rational, Hello} // imports both Rational and Hello
// Wildcard imports
import week3._                 // imports everything in package week3
```

### Automatic Imports

Some entities are automatically imported by Scala.
* All members of package `scala`
* All members of package `java.lang`
* All members of the singleton object `scala.Predef`.

This allows us to the following directly:
```
Int      scala.Int
Boolean  scala.Boolean
Object   java.lang.Object
require  scala.Predef.require
assert   scala.Predef.assert
```

### Traits

Traits are similar to abstract classes where they contain unimplemented and implemented methods (concrete method definitons). 
```scala
trait Planar {
    def height: Int
    def width: Int
    def surface = height * width
}
```
Traits vs Abstract Classes:
* Classes, objects and traits can inherit from at most one class or abstract class but many traits. Eg
``` scala
class Square extends Shape with Planar with Movable ...
```
* Abstract classes can have constructor value parameters as well as type parameters (i.e. generic type parameters). Traits can have only type parameters. 
* Abstract classes are fully interoperable with Java. You can call them from Java code without any wrappers. Traits are fully interoperable only if they do not contain any implementation code

# Scala Class Hierarchy

![Scala Class Hierarchy](https://github.com/rohitvg/scala-principles-1/blob/master/resources/images/scala_class_hierararchy.png) 

### Any
`scala.Any` is the top superclass in the scala higherarchy. This is the class that defines:
*  `==`
* `!=`
* `toString()` 
* `hashCode()`
It has 2 subclasses: `scala.AnyVal`,`scala.AnyRef`. 

`scala.AnyVal` is the superclass of all the primitive/value types i.e. Int, Boolean, etc. 

`scala.AnyRef` is the superclass of all the other classes. If Scala is used in the context of a Java runtime environment, then `scala.AnyRef` corresponds to java.lang.Object.

### Nothing and Exception

`Nothing` type is at the bottom of Scala’s type hierarchy. It is a subtype of every other type. There is no value of type Nothing.
Why is that useful?
* To signal abnormal termination (eg. return Nothing.)
* As an element type of empty collections.

`Exception` type: Scala exception handling is similar to Java. The expression:
```scala
throw Exc
```
aborts evaluation by throwing Exception Exc.
The type of `throw Exc` is `Nothing`.

### Null

Every reference class type also has `null` as a value. The type of null is `Null`.

`Null` is a subtype of every class that inherits from Object; it is **incompatible** with subtypes of AnyVal.
```scala
val x = null         // x: Null
val y: String = null // y: String
val z: Int = null    // error: type mismatch
```

### Question
What is the type of:
```scala
if (true) 1 else false

// options
- Int
- Boolean
- AnyVal
- Object
- Any
```
Both 1 and false have `AnyVal` as the first common baseclass. Hence `AnyVal`.