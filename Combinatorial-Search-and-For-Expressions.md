Here we will see how to handle nested-sequences.

The calculations which are usually expressed in loops can be expressed using higher order functions in sequences.<br/>
Eg. Given a positive integer n, find all pairs of positive integers i and j, ith 1 <= j < i < n such that (i+j) is prime. So if n = 7, we would get the below pairs:
```
i     |  2  3  4  4  5  6  6
j     |  1  2  1  3  2  1  5
------------------------------
i + j |  3  5  5  7  7  7  11
```