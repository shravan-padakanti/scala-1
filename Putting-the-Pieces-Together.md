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

### Solution

```scala
package example

import scala.io.Source

object Phone {

  val in = Source.fromURL("https://raw.githubusercontent.com/rohitvg/scala-principles-1/master/resources/data/linuxwords.txt")

  // In the list of words, filter such that for each word, each character is a letter (since our map only consists of letters and no special characters.
  var words = in.getLines.toList.filter(word => word.forall(chr => chr.isLetter))

  val mnemonics = Map('2' -> "ABC", '3' -> "DEF", '4' -> "GHI", '5' -> "JKL", '6' -> "MNO", '7' -> "PQRS", '8' -> "TUV", '9' -> "WXYZ")

  // Map each letter to its digit
  val charCode: Map[Char, Char] = for (
    (digit, letters) <- mnemonics;
    letter <- letters
  ) yield (letter -> digit)

  // Map a word to the digit string
  def wordCode(word: String): String = word.toUpperCase.map(charCode)

  // Map from digit string to respective possible words
  def numToWord: Map[String, Seq[String]] = {
    words.groupBy(wordCode) withDefaultValue (Seq()) // withDefaultValue (Seq()) is used so that if user tries to encode digits the words for which are not contained in our list, we do not get an exception.
  }

  // Function to encode a number as a list of all possible words
  def encode(numberStr: String): Set[List[String]] = {
    if (numberStr.isEmpty) Set(List())
    else {
      for {
        split <- 1 to numberStr.length
        word <- numToWord(numberStr.take(split))
        rest <- encode(numberStr.drop(split))
      } yield {
        word :: rest
      }
    }.toSet
  }

  // Test!
  encode("7225247386")

}
```