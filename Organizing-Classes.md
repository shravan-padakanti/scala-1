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
