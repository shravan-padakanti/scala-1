So far we saw functions that could be applied to list which were singler order i.e they took in type or value params and returned a type or a value result. Here will see functions that can be applied to a list that will be higher order.

### Applying a Function to Elements of a List

A common operation is to transform each element of a list and then return the list of results.
Eg. to multiply each element of a list by the same factor, we could write:
```scala
def scaleList(xs: List[Double], factor: Double): List[Double] = xs match {
	case Nil => xs
	case y :: ys => y * factor :: scaleList(ys, factor)
}
```

### Mapping

In similar fashion, **method** `map` of the `List` class, which can apply an arbitrary operation to all elements of a list can be defined as:
```scala
abstract class List[T] { ...
    def map[U](f: T => Y): List[U] = this match {
        case Nil => this
        case x :: xs => f(x) :: xs.map(f)
    }
}
```
> In fact, the actual definition of `map` is a bit more complicated because it is tail-recursive, and also because it works for arbitrary collections, not just lists.

Here's a much more concise `scaleList` re-written using `map`:
```scala
def scaleList(xs: List[Double], factor: Double) = xs map (x => x * factor)
```

### Filtering

Another common operation is filtering all the elements in a list that satisfy a given condition.

```scala
abstract class List[T] {
	...
	def filter(p: T => Boolean): List[T] = this match {
		case Nil => this
		case x :: xs => if (p(x)) x :: xs.filter(p) else xs.filter(p)
	}
}
```

### Variations of filter
```scala
xs filter p     // The list consisting of those elements of xs that satisfy the predicate p.
xs filterNot p	// The list consisting of those elements of xs that do not satisfy the predicate p.
xs partition p  // Split xs into a pair of lists; one with elements that satisfy the predicate p, the other with elements that do not, giving the pair of lists `(xs filter p, xs.filterNot p)`
xs takeWhile p  // The longest prefix of elements in the list that all satisfy p (basically 'take' elements 'while' p is true).
xs dropWhile p  // The list without the longest prefix of elements that all satisfy p (basically 'drop' elements 'while' p is true).
xs span p       // Split xs according to a predicate, giving the pair of collections `(xs takeWhile p, xs.dropWhile p)`, computed in a single traversal.
```
### Example

Write a function to "pack" same elements of a list into a new list, and then get a list of elements and their count:
Eg. 
```scala
pack( List("ababacbc".toList") ) 
``` 
should give
```scala
List( List('a','a','a'), List('b','b','b'), List('c','c') )
List( ('a',3), ('b',3), ('c',2) )
```
Solution:
```scala
def pack[T]( list: List[T] ): List[List[T]] = xs match {
    case Nil => Nil
    case y :: ys => {                                  // y is the head, ys is the tail
        val (first, rest) = ys span (arg => arg == y)  // arg is all the elements of the list, 
        first :: pack(rest)
    }
}

def encode[t]( list: List[T] ): List[(T, Int)] = {
     pack(list) map ( arg => (arg.head, arg.length))   // Note that arg will be element of the list - also a list eg. List('a','a','a')
}
```