# Basic Types

We've seen a couple of Scala's types (`Int`, `Double` and `String`), but before we go much further, let's look at the other basic types that Scala gives us to play with. Create some val objects in the REPL:

~~~~~~~~
scala> val n = 3
n: Int = 3

scala> val m = 3.3
m: Double = 3.3

scala> val s = "abc"
s: String = abc

scala> val c = 'c'
c: Char = c

scala> val l = 10000L
l: Long = 10000

scala> val f = 2.4F
f: Float = 2.4

scala> val b = true
b: Boolean = true

scala> val bt: Byte = 4
bt: Byte = 4

scala> val sh: Short = 10
sh: Short = 10

scala> val hex = 0x11
hex: Int = 17
~~~~~~~~

The fourth `val`, `c`, is a single character, represented by a `Char`. As in many other C-family languages, character literals use single quotes, while string literals use double quotes.

Using an `L` suffix on an integer literal denotes a long integer, one that has a much larger range than a plain Int. Using an F suffix on a floating point literal denotes a `Float`, a floating-point type that has a smaller range than a `Double`. You'll find that you use `Double` almost all the time when working with floating point values.

The `Boolean` type can only take two values, either true or false.

There are two other types that are both kinds of integer, and you have to tell Scala that you want to use these rather than a plain `Int`. As you might expect, you use the colon syntax to tell the compiler that you want (for example) `bt` to be a `Byte`. So `Byte` is, as you'd expect, a single byte, while `Short` is an integer type that is smaller than an `Int`. We don't use `Short` very much.

The last example shows a different form of integer literal, specifying a hex value with a leading `0x`. The compiler still recognizes this as an `Int`, though.

Note that, unlike languages such as C and C++, Scala doesn't support unsigned integer types.

## Exercise

Find out the ranges of the various types we've mentioned. For example, a `Byte` can hold values from -127 to 126.