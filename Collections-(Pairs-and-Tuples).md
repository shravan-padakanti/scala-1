### Sorting Lists Faster

Previously we saw [Insertion Sort](https://github.com/rohitvg/scala-principles-1/wiki/Collections-(Lists)#sorting-lists). Here is **Merge Sort**:

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
```

let's look at an implementation of `merge`:

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
