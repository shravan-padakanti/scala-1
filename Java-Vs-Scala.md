### Object Orientation

Java is not pure Object Oriented: It has **Primitive Types** and **Reference Types**. Eg. `System.out.println(1.toString)` would fail.

In Scala,  everything is a **Reference Type**. So `println(1.toString)` works. Scala uses the correct type at compile time when generating bytecode i.e. in this case it either generates an `Int` object or a Java `int` in bytecode.

### Final

In Java you have to declare specific variables that you do not want to change as `final`.

In Scala, by default variables defined using `val` or `def` are final. You can use `var` to define variables that can change.

### Singletons and Static/Class variables

In Java you have to implement Singleton manually, also to keep track of certain things (eg. instance count, etc) you have to use `static` variables or methods.

In Scala, a singleton can be defined by just writing a regular class with the keyword `object` instead of `class`.
Also if you have a class of which you want to keep track of certain things (eg. instance count, etc), then just declare an `object` with the same name as that of the class. In this case, the object becomes the "**companion object**" of the class. All definitions inside this object will be static by default i.e. these will be Class Variables.
```scala
class Car {
}
object Car {
    def someMethod = { ... }
    val someString = "Hello"
}
// usage:
def c1 = new Car
def c2 = new Car
println(Car.someString)
```

### Classname Convention
Java requires classname and filename to be the same.

Scala does not have this requirement. Only requirement is a class and its companion object be defined in the same file.
