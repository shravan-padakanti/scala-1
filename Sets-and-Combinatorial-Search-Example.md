So far we have seen forms of `Seq`. Here we will see **Sets**. 

**Sets** Sets are another basic abstraction in the Scala collections are very close to Sequences.
A set is written analogously to a sequence:
```scala
val fruit = Set(”apple”, ”banana”, ”pear”)
val s = (1 to 6).toSet
```
Most operations on sequences are also available on sets:
```
s map (_ + 2)
fruit filter (_.startsWith == ”app”)
s.nonEmpty
```
(see [Iterables Scaladoc](http://docs.scala-lang.org/overviews/collections/sets.html) for a list of all supported operations) 

### Sets vs Sequences

The principal differences between sets and sequences are:
1. Sets are **unordered**; the elements of a set do not have a predefined order in which they appear in the set
2. sets **do not have duplicate elements**:
``` scala
s map (_ / 2) // Set(2, 0, 3, 1)
```
3. The fundamental operation on sets is **contains**: 
```scala 
s contains 5 // true
```

## Example: N-Queens

### Problem
The eight queens problem is to place eight queens on a chessboard so that no queen is threatened by another.
* In other words, there can’t be two queens in the same row, column, or diagonal.
We now develop a solution for a chessboard of any size, not just 8. <br/>
Eg. 
```
      0      1     2     3
   -------------------------
0  |     |  X  |     |     |
   -------------------------
1  |     |     |     |  x  |
   -------------------------
2  |  x  |     |     |     |
   -------------------------
3  |     |     |  X  |     |
   -------------------------
```
One way to solve the problem is to place a queen on each row. When we have placed k - 1 queens, one must place the kth queen in a column where it’s not “in check” with any other queen on the board. Eg. 0th queen doesn't matter. So if the first quen is placed as above in the (0,1) the 2nd goes in (1, 3), and so on.

### Algorithm

We can solve this problem with a recursive algorithm:
* Suppose that we have already generated all the solutions consisting of placing `k-1` queens on a board of size `n`.
* Each solution is represented by a list (of length `k-1`) containing the numbers of columns (between `0` and `n-1`).
* The column number of the queen in the `k-1`the row comes first in the list, followed by the column number of the queen in row `k-2`, etc.
* The solution set is thus represented as a set of lists, with one element for each solution.
* Now, to place the `k`th queen, we generate all possible extensions of each solution preceded by a new queen:

```
      0      1     2     3
   -------------------------
0  |     |  X  |     |     |
   -------------------------
1  |     |     |     |  x  |
   -------------------------
2  |  x  |     |     |     |
   -------------------------
3  |     |     |  X  |     |
   -------------------------
```
So using he algorithm above, the solution using the first 3 queens would be `list(0, 3, 1)`because we placed:
1. 3rd queen in (2, 0)
1. 2nd in (1, 3)
1. 1st in (0,1)
So placing the 4th queen, our solution will be `list(1, 0, 3, 1)`

### Implementation:
```scala
def queens(n: Int) = {
def placeQueens(k: Int): Set[List[Int]] = {
  if (k == 0) Set(List())
  else
  for {
    queens <- placeQueens(k - 1)
    col <- 0 until n
    if isSafe(col, queens)
    } yield col :: queens
  }
  placeQueens(n)
}
```
If `k == 0` return empty list. Now, in the case where `k` is greater than 0 we have to do some real work. In general, to place `k` queens, we have to solve the problem of placing `k - 1` queens. We'll let `queens` range over the set of our partial solutions returned by `placeQueens(k - 1)`.

Next, we have to put our `k` queen into a certain column. We can simply try all the possible columns - `col <- 0 until n`. We can't place the queen in any column we please, we need to make sure it doesn't threaten any other queen. So, we'll put a filter in there, that says that the column for the queen is safe with respect to the previous queens. (`isSafe(col, queens))`

If it is, then we can yield a new solution, which will be our partial solution `col`, augmented by the queen in the new column. So it would be `col :: queens`.

Next we implement `isSafe`:
```scala
def isSafe(col: Int, queens: List[Int]): Boolean = {
  val row = queens.length
  val queensWithRow = (row - 1 to 0 by -1) zip queens
  queensWithRow forall {
    case (r, c) => col != c  && math.abs(col - c) != row - r
  }
}
```
It takes a column to place the new queen and an existing solution until this point, and returns a boolean indicating if it is safe to place the queen in the given column. The row assumed is the next row.

The first thing we want to do is add rows to all the queens we look at here; the row of the queen to be placed will be just `queens.length`, since the other queens are in rows 0 to `n - 1`.

Next, we want to add a row to each of our previous queens, transforming our list of `Int`s into a list of pairs, of row and column. We've got a set of solutions, which is something like `List(0, 3, 1)`, with the columns of the previous queens. We want to transform that into a solution that adds the rows. The first element was actually the last row to be placed, so we'd get `List((2,0), (1,3), (0,1))`.

The idea here is to use a `zip` with a range - the range that we want to apply here is the range that goes from row - 1 to 0, by -1 steps. We zip that sequence with the list of our queens, and we call that `withRow`. So now we've got the partial solution of queens, represented with rows.

Now what we can do is simply check whether the ween at `row` and `column` is in check with any of our `queensWithRow`. `forall` of these `queensWithRow`, it must be that the new queen is not in check ...

So we take the row and the column out of the pair with a `case` statement, and now comes our check. What do we need to check? First, we need to make sure that the current column is not the same as any of the previous queen's columns. `col != c`

We also need to make sure that the queen is not in check over any of the diagonals. Meaning, the absolute difference between the two columns (`math.abs(col - c)`) must not be the same as the absolute difference between the two rows (`row - r`).

If that predicate is true, then we know that the queen is not in check over any of the diagonals with the queen in `(r, c)`!
