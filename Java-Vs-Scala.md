1. Java is not pure Object Oriented: It has **Primitive Types** and **Reference Types**.
Eg. `System.out.println(1.toString)` would fail. 
In Scala,  everything is a **Reference Type**. So `println(1.toString)` works. Scala uses the correct type at compile time when generating bytecode i.e. in this case it either generates an `Int` object or a Java `int` in bytecode.