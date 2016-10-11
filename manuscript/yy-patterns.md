# Pattern Matching and Case Classes

You saw how to use `match` expressions in chapter 7 when we talked about flow control, but `match` is much  more powerful than it may appear, and that is the subject of this chapter.

The `match` keyword is used to implement *pattern matching*, which allows you perform operations based on matching patterns in data.

## Pattern Matching

If you have programmed in another functional language you may well have come across pattern matching, as it is supported by many functional languages, including OCaml, F# and Clojure.

As it's name implies, pattern matching matches a *selector* against one or more patterns, and executes the expression associated with the first one that matches.

-- Let's recap the simple example you saw earlier:

~~~~~~~~
scala> val n = 3
n: Int = 3

scala> n match {
     |   case 1 => println("It's one")
     |   case 2 => println("It's two")
     |   case 3 => println("It's three")
     |   case _ => println("Some other number")
     | }
It's three
~~~~~~~~

In its simplest form, a `match` is a simple multi-way choice based on value, with the underscore acting as a wildcard. The value we are matching against (`n` in this case) is called the *selector*, and since the case values are constant, they are called *constant patterns*.

The expression doesn't have to be on a single line or end with a special terminator, because, as usual, the compiler is smart enough to figure out what belongs to the expression. That means that you can write something like this:

~~~~~~~~
     |   case 3 => 
     |      println("It's three")
     |      println("Yes, definitely three")
~~~~~~~~

### Matching on Types

You can match on type as well as on value, as shown in this example:

~~~~~~~~
scala> def testMatch(a: Any) = a match {
     |   case n: Int => println("It's an Int")
     |   case b: Boolean => println("It's a Boolean")
     |   case s: String => println("It's a String")
     |   case _ => println("Some other type")
     | }
testMatch: (a: Any)Unit
~~~~~~~~

The argument is an `Any`, in most cases not the most useful type for a function argument, but it allows us to show how `match` can be used with types.

Each of these cases is a *typed pattern*, and if the value matches the type, the expression on the right side of the arrow will be evaluated.

Each of the typed patterns specifies a variable name. This is partly because the syntax of Scala requires it, but mainly because you can use the variable in the matching expression:

~~~~~~~~
scala> def testMatch(a: Any) = a match {
     |   case n: Int => println(s"It's an Int, value $n")
     |   case b: Boolean => println(s"It's a Boolean, value $b")
     |   case s: String => println(s"It's a String, value $s")
     |   case _ => println("Some other type")
     | }
testMatch: (a: Any)Unit
~~~~~~~~

If the value passed to `testMatch` is an `Int`, it will match the first pattern and be bound to `n`. Note how string interpolation makes it simple to use pattern variables in expressions.

T> Make sure you don't use the same name for the selector and a case variable.

The default case uses a wildcard and so you can't use the value, but if you do want to, simply replace the underscore with a variable name:
 
~~~~~~~~
     |   case o => println(s"Some other type: $o")
~~~~~~~~

And if you don't want to do anything with the default, just omit an expression:

~~~~~~~~
     |   case _ =>
~~~~~~~~

### Matching on Arguments
Note how we are matching on the argument to the function. This is a very common way to use pattern matching.

Sometimes you might want to match on multiple arguments at once, but you can only match on one item. The secret is to bundle the arguments together into a *tuple*, like this:

~~~~~~~~
scala> def matchTwo(a: String, b: String) = (a, b) match {
~~~~~~~~

> The construct `(a, b)` defines a 2-tuple (aka pair), a simple immutable container holding two values, which can be of different types. Tuples can hold from 1 to 22 values, and are represented by a comma-separated list in parentheses. They can also be useful when you want to return more than one value from a function.

Within the `match` provide cases for the specific tuples you want to handle:

~~~~~~~~
scala> def matchTwo(a: String, b: String) = (a, b) match {
     | case ("abc", "def") => println("Matched abc and def")
     | case ("abc", _) => println("Matched abc and _")
     | case (_, "def") => println("Matched _ and def")
     | case (_, _) => println("Matched _ and _")
     | }
matchTwo: (a: String, b: String)Unit

scala> val s1 = "abc"
s1: String = abc

scala> val s2 = "xyz"
s2: String = xyz

scala> matchTwo(s1, s2)
Matched abc and _
~~~~~~~~

### Using Guards

## Case Classes
Case classes allow pattern matching on objects without requiring a lot of boilerplate code.

### Adding unapply To Classes

What if you want to use a class with pattern matching, but it cannot be a case class because it is part of an inheritance hierarchy? When this happens, you can implement a method called `unapply`, which is what makes pattern matching work for case classes.
