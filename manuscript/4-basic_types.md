-# Part 2 - The Basics

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

For more details on the basic types, see the [Scala Language Specification](http://scala-lang.org/files/archive/spec/2.11/).

## Characters
You have seen how character literals are enclosed in single quotes. Scala also supports the usual escape sequences, familiar to anyone who has used a C-family language, such as `\t` for tab and `\n` for newline. If you're not familiar with these, you'll need to remember to use a double slash if you want a literal slash. In this example, trying to give a Windows directory name results in a tab character being generated:

~~~~~~~~
scala> val s1 = "The directory is c:\temp"
s1: String = The directory is c:	emp

scala> val s1 = "The directory is c:\\temp"
s1: String = The directory is c:\temp
~~~~~~~~

Since Scala uses Unicode, you can use escape sequences to represent characters that you may not be able to type, eg. `\u0014` for `A`.

While we're thinking about Unicode, you may see some unusual symbols in Scala code. There are several operators that are usually typed as two characters. Two of these, `=>` and `<-` can be represented by their Unicode equivalents, which are 
`\u21D2` ‘⇒’ and `\u2190` ‘←’. Whether or not you think this is useful is a personal choice!
 
## Strings
Under the hood, Scala strings are Java strings, which means that string literals are enclosed in double quotes, and that they support all the usual operations you'd expect.

Scala has, however, given them some extra functionality, some of which we'll meet here, and some later.

The first extension is the ability to create multi-line strings, using `"""`. These strings can contain embedded newline characters, and don't interpret the usual escape characters:

~~~~~~~~
scala> val s3 = """
     | A multi-line
     | string with \t in it
     | """
s3: String =
"
A multi-line
string with \t in it
"
~~~~~~~~

A second, really useful extension is string interpolation, which gives you the ability to perform operations on strings. Scala provides the *simple interpolator*, which will substitute values into strings.

~~~~~~~~
scala> val x = 10
x: Int = 10

scala> val s4 = s"x is $x"
s4: String = x is 10
~~~~~~~~

The `s` prefix specifies the interpolator, and you refer to values within the string by prefixing their name with a `$`

T> If you want to use a literal dollar sign in an interpolated string, double it. For example, `s"Cost in dollars is $$$cost"`

There is also a formatting interpolator, which gives you the ability to create formatted strings

~~~~~~~~
scala> val item = "Book"
item: String = Book

scala> val cost = 24.99
cost: Double = 24.99

scala> val s5 = f"$item%s costs $cost%2.2f"
s5: String = Book costs 24.99
~~~~~~~~

The `f` prefix introduces the interpolator, `$` is used to reference values, and these are followed by a `%` with formatting information. This interpolator makes use of Java's string formatting, and you can find details of the format strings in the description of the [`Formatter` class](https://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html)

## Exercise

Find out the ranges of the various types we've mentioned. For example, a `Byte` can hold values from -127 to 126.