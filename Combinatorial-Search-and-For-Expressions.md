Here we will see how to handle nested-sequences.

The calculations which are usually expressed in loops can be expressed using higher order functions in sequences.<br/>
Eg. Given a positive integer n, find all pairs of positive integers i and j, ith 1 <= j < i < n such that (i+j) is prime. So if n = 7, we would get the below pairs:
```
i     |  2  3  4  4  5  6  6
j     |  1  2  1  3  2  1  5
------------------------------
i + j |  3  5  5  7  7  7  11
```
In a conventional way, we would use 2 for loops which are nested, 1 for i and 1 for j. How would you do this in functional programming?

### Algorithm:

A natural way to do this is to:
* Generate the sequence of all pairs of integers `(i, j)` such that `1 <= j < i < n`.
* Filter the pairs for which `i + j` is prime.

One natural way to generate the sequence of pairs is to:
* Generate all the integers i between 1 and n (excluded).
* For each integer i, generate the list of pairs `(i, 1), ..., (i,i-1)`.

This can be achieved by combining until and map:
```scala
(1 until n) map (i =>
(1 until i) map (j => (i, j)))
```