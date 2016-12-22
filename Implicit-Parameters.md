**Problem:** How to parameterize `msort` [we saw previously](https://github.com/rohitvg/scala-principles-1/wiki/Collections-(Pairs-and-Tuples)#merge-sort-using-pairs) so that it can also be used for lists with elements other than `Int`s? 
```scala
def msort(xs List[T]): List[T] = {
```
Does not work because we use comparison operator `<` in `merge` to compare elements while merging, and that operator is not defined for a generic type T.

**Idea**: So pass the comparison function as a parameter i.e. parameterize `merge` using the comparison function.
```scala
def msort[T](xs: List[T])(lt: (T, T) => Boolean)= {
    ...
    merge(msort(fst)(lt), msort(snd)(lt))
    ...
    def merge(cs: List[T], ys: List[T]) = (xs, ys) match {
        ...
        case (x :: xs1, y :: ys1) =>
            if (lt(x, y)) ...                 // use function lt which we got from the parameter
            else ...
    }
}
```
This can be used like:
```scala
val nums = List(2, -4, 5, 7, 1)
msort(nums)((x, y) => x < y)

val fruits = List("apple", "pineapple", "orange", "banana")
msort(fruits)((x, y) => x.compareTo(y) < 0)
```

### Parametrization with Ordered

There is already a class in the standard library that represents orderings: `scala.math.Ordering[T]`. It provides ways to compare elements of type T. 

So instead of passing a comparison function as a parameter, we can pass `Ordering[T]` as a parameter:
```scala
def msort[T](xs: List[T])(ord: Ordering)= {
    ...
    merge(msort(fst)(lt), msort(snd)(lt))
    ...
    def merge(cs: List[T], ys: List[T]) = (xs, ys) match {
        ...
        case (x :: xs1, y :: ys1) =>
            if (ord.lt(x, y)) ...                 // use ordering compare function
            else ...
        ...
        merge(msort(fst)(ord), msort(snd)(ord))   // pass ord recursively
    }
}
```
Usage:
```scala
import math.Ordering

msort(nums)(Ordering.Int)
msort(fruits)(Ordering.String)
```

### Implicit Parameters
As seen above, Passing around `lt` or even `ord` values is cumbersome. We can avoid this by making `ord` an **implicit parameter** by declaring the parameter as `implicit`. The compiler then figures out the right implicit to pass based on the demanded type.

```scala
def msort[T](xs: List[T])(implicit ord: Ordering)= { //(implicit ord: Ordering)
    ...
    merge(msort(fst)(lt), msort(snd)(lt))
    ...
    def merge(cs: List[T], ys: List[T]) = (xs, ys) match {
        ...
        case (x :: xs1, y :: ys1) =>
            if (ord.lt(x, y)) ...                 // use ordering compare function
            else ...
        ...
        merge(msort(fst), msort(snd))             // no need to pass ord
    }
}
```
Usage
```scala
msort(nums)
msort(fruits)
```

### Rules for Implicit Parameters
Say, a function takes an implicit parameter of type T. The compiler will search an implicit definition that
* is marked implicit
* has a type compatible with T
* is visible at the point of the function call, or is defined in a companion object associated with T.
If there is a single (most specific) definition, it will be taken as actual argument for the implicit parameter.
Otherwise it’s an error.