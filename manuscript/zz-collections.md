# Collections

Functional programming tends to use standard collections, such as `Map` and `List`, rather than creating custom classes.

-- strings and arrays as collections

-- brief work about arrays, esp () syntax

-- mutable and immutable

-- diagram of the collection hierarchy
[The Collections Hierarchy](images/Scala_Collections.png)

## Arrays and Ranges

You have seen arrays used in the `main` method of Scala applications:

~~~~~~~~
def main(args: Array[String])
~~~~~~~~

`Array` is a generic class, and is bounds checked. You can create arrays using `apply` on the `Array` companion object:

~~~~~~~~
scala> val strings = Array("abc", "123", "xyz")
strings: Array[String] = Array(abc, 123, xyz)

scala> val s = strings(0)
s: String = abc

scala> val s2 = strings(3)
java.lang.ArrayIndexOutOfBoundsException: 3
~~~~~~~~

Note that (unlike most other C-family languages) Scala uses round brackets for array element access. This is for two reasons: first, square brackets are used for generics, and second, using parentheses on an object actually means calling its `apply` method, so in the example we are actually calling `strings.apply(0)`.

## Sequences
-- introduce the idea of `Seq`

## Lists

### Creating Lists

### Functional Operations on Lists

## Maps

