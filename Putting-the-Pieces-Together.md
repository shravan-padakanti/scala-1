Here we will see a larger example where we use the concepts we have learned so far.

### Problem

Phone keys have mnemonics assigned to them.
```scala
val mnemonics = Map( '2' -> "ABC", '3' -> "DEF", '4' -> "GHI", '5' -> "JKL", 
                     '6' -> "MNO", '7' -> "PQRS", '8' -> "TUV", '9' -> "WXYZ")
```
Assume you are given a dictionary words as a list of words. Design a method:
```scala
translate(phoneNumber)
```
which produces all phrases of words that can serve as mnemonics for the phone number.

**Example:** The phone number `7225247386` should have the mnemonic Scala is fun as one element of the set of solution phrases.