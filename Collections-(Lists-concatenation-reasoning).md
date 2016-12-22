# Reasoning behind List Concatenations

**Q**: How can we prove that the concatenation using ++ or the ::: operator is associative, and that it allows using Nil as an element either on the left or the right of it?

**A**: Done using **Structural Induction**: (prove that it holds for base case p(Nil), and for a lists `xs` and element `x`, if `p(xs)` holds, then for `p(x :: xs)` also holds.<br/>
This is analogus to **Natural induction**: (prove that it holds for p(base case) and if it holds for p(n) then it also holds for p(n+1).

This and the next lecture are mathematical proofs. Not required for programming in Scala, or for any assignments.