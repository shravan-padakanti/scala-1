# List Methods

All the methods can be found here: 
http://www.scala-lang.org/api/2.12.x/scala/collection/immutable/List.html

###Sublists and element access:

* `xs.length` : The number of elements of `xs`.
* `xs.last`   : The lists's last element, exception if `xs` is empty
* `xs.init`   : A list consisting of all elements of `xs` except the last one, exception if `xs` is empty
* `xs take n` : A list consisting of the first `n` elements of `xs`, or `xs` itself if it is shorter than `n`
* `xs drop n` : The rest of the collection after taking `n` elements
* `xs(n)`		: The element of `xs` at index `n`

###Creating new lists:

* `xs ++ ys`        : the list consisting of all elements of `xs` followed by all elements of `ys`
* `xs.reverse`      : the list containing the elements of `xs` in reversed order
* `xs.updated(n,x)` : returns a lit that contains all the elements of `xs`, except at the given index, where it contains `x`. (Since lists are immutable, it returns a new list).

###Finding elements

* `xs indexOf x`  : the index of the first element in `xs` equal to `x`, or -1 if `x` does not appear in `xs`
* `xs contains x` : the same as `xs indexOf x >= 0`

###Implementation Issues

We know that the complexity of `head` is a simple field selection; it's a very small, **constant time**. Can `last` be implemented as efficiently? `tail` again is just a simple selection of a field of a list, ie constant time; can `init` be implemented as efficiently?

## Some implementations of List functions:

### last

```scala
def last[T](xs: List[T]): T = xs match {
    case List() => throw new Error("last of empty list")
    case List(x) => x
    case y :: ys => last(ys)
}
```

So `last` takes steps proportional to the length of the list `xs` - we need to take one recursion for each element in the list.

### init

```scala
def init[T](xs: List[T]): List[T] = xs match {
    case List() => throw new Error("init of empty list")
    case List(x) => List()
    case y :: ys => y :: init(ys)
}
```

### concatenation

How can concatenation be implemented? So far, everything we've done has been with a pattern-match on the list in question - now there are *two* lists. oh man... which list should we pattern match on? In this case, the first element of the result list here clearly depends on `xs` - so it makes sense to match on that.

```scala
def concat[T](xs: List[T], ys: List[T]) = xs match {
    case List() => ys
    case z :: zs => z :: concat(zs, ys)
}
```

What is the complexity of `concat`? Well, it's clear that we'll need a call of `concat` for each element of the left list, so complexity will correspond to the length of the list `xs`.

### reverse

```scala
def reverse[T](xs: List[T]): List[T] = xs match {
    case List() => xs //return empty list as is
    case y :: ys => reverse(ys) ++ List(y)
}
```

What's the complexity of `reverse`? Well, we know that concatenation is linear, proportional to the size of the list on the left hand size of the operator. That list in this case is a list that grows from 1 to the length of `xs`.

Furthermore, we do one step for each element in the reversed list, because we go through each element of `ys` and put it at the end of the reversed list.

So, taken together, that gives us a quadratic complexity of **N * N**, which is a bit disappointing. We all know that with an array or a mutable linked list of pointers that we could reverse in linear time - we'll see later on how we might do better.

## Sorting Lists Faster

Previously we saw [Insertion Sort](https://github.com/rohitvg/scala-principles-1/wiki/Collections-(Lists)#sorting-lists). Here is **Merge Sort** (`O(nlogn)`):

Idea: 
* If the list consists of zero or one elements, it is already sorted.
* Else, separate the list into two sub-lists, each containing around half the elements of the original list
* Sort the two sub-lists
* Merge the two sorted sub-lists into a single sorted list

```scala
def msort(xs List[Int]): List[Int] = {
	val n = xs.length / 2
	if (n == 0) xs
	else {
		def merge(xs: List[Int], ys: List[Int]) = ???
		val (fst, snd) = xs splitAt n
		merge(msort(fst), msort(snd))
	}
}

// Will be improved later!!
def merge(xs: List[Int], ys: List[Int]) = xs match {
	case Nil => ys
	case x :: xs1 => ys match {
		case Nil => xs
		case y :: ys 1 => {
			if (x < y) x :: merge(xs1, ys)
			else y :: merge(xs, ys1)
		}
	}
}
```
This implementation of `merge` has nested pattern match, and also it does not reflect the symmetry of the merge sort algorithm. A better version will be show using pairs with pattern matching later.

### splitAt function

The `splitAt` function on lists returns two sublists - the elements up to the given index, and the elements from that index. The lists are returned in a *pair*.

