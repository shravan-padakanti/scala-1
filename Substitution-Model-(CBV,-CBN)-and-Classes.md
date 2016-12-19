We are familiar with the substituion model, and how expressions with simple datatypes are evaluated using CBV/CBV.

Suppose we have a class definition as follows:
```scala
class C(x1, ..., xm){ ... def f(y1, ..., yn) = b ... }
```

**Question**: How is an instantiation of the class `new C(e1, ..., em)` evaluted? <br/>
**Answer**: The expression arguments `e1, ..., em` are evaluated like the arguments of a normal function. That’s it. The resulting expression, `new C(v1, ..., vm)`, is already a value.

**Question**:  How is an function call on an instantiation evaluated: `new C(v1, ..., vm).f(w1, ..., wn)` <br/>
**Answer**: The expression `new C(v1, ..., vm).f(w1, ..., wn)` is rewritten to:
```
[w1/y1, ..., wn/yn][v1/x1, ..., vm/xm][new C(v1, ..., vm)/this] b    // Here '/' indicates 'replaced by'
```
There are three substitutions at work here:
* the substitution of the formal parameters `y1, ..., yn` of the function f by the arguments `w1, ..., wn`
* the substitution of the formal parameters `x1, ..., xm` of the class C by the class arguments `v1, ..., vm`
* the substitution of the self reference this by the value of the object `new C(v1, ..., vn)`.