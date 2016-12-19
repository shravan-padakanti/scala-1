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
