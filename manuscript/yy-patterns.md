# Pattern Matching and Case Classes

You saw how to use `match` expressions in chapter 7 when we talked about flow control, but `match` is much  more powerful than it may appear, and that is the subject of this chapter.

The `match` keyword is used to implement *pattern matching*, which allows you perform operations based on matching patterns in data.

## Pattern Matching

-- supported by many functional languages

-- recap basic example

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

-- match on types

~~~~~~~~
scala> def testMatch(a: Any) = a match {
     |   case n: Int => println("It's an Int")
     |   case b: Boolean => println("It's a Boolean")
     |   case s: String => println("It's a String")
     |   case _ => println("Some other type")
     | }
~~~~~~~~

Note how we are matching on the argument to the function. This is a very common pattern.

-- match on multiple arguments

### Using Guards

## Case Classes