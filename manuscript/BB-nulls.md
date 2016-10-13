# Nulls and Exceptions

## Handling Nulls

Developers rapidly become used the concept of 'null' and having to handle it in code and in database queries. It would be unusual to examine a codebase written in C, C++, C# or Java and not find lines like this:

~~~~~~~~
if (obj != null)
  obj.doSomething();
~~~~~~~~

Null is a ubiquitous concept, but it is one that causes a lot of problems.

### What's Wrong With Null?

Tony Hoare, the British computer scientist, added the idea of null to the type system he designed for Algol W in the 1960s, and has since called it his "billion dollar mistake"[^1].

[^1]: https://en.wikipedia.org/wiki/Tony_Hoare

Nulls are a problem for several reasons.

* Null conveys no information
* It breaks the type system, because it is a nothing, a black hole
* Because it breaks the type system, you can't tell from a function signature whether a function may end up handing you back a null
* Developers have to keep checking for null references or pointers to avoid code blowing up

### The Null Type
In order to build a robust type system, Scala has a `Null` type that has only one instance, `null`.

`Null` is what is known as a *bottom type*, because it appears at the bottom of the type hierarchy. It is a subclass of every reference type, and so `null` can be assigned to any value that inherits from `AnyRef`.

But even though Scala defines `null`, it isn't used very often. How we deal with nulls is dealt with in the following section.

### Handling Nulls
If a value could be null, it would be very useful to be able to see this in function signatures. For example, suppose that a `getCustomerDetails(customerId: Int)` function may return a null if no such customer exists. Simply returning a `Customer` reference does not tell us this, and the developer has to check.

Scala has introduced the `Option[T]` type to deal with this. An `Option[T]` (which I'll refer to simply as `Option` from now on) is a type that may contain one of two things: a value, or a marker showing that there is no value.

> `Option` is a *nullable type*, and such types are becoming increasingly common. Haskell has `Maybe`, Java 8 has `Optional<T>`, C# has `Nullable<T>` and C++ has `std::optional`. Nullable types, although simple in concept, have far reaching consequences for the way in which you write code.

`Option[T]` has two subtypes: `Some[T]` to hold a value of type `T`, and `None` to show that there isn't a value.

### Using `Option[T]`

Let's start with a simple example:

~~~~~~~~
scala> val data = Some(3.3)
data: Some[Double] = Some(3.3)

scala> val noData = None
noData: None.type = None

scala> def test(o: Option[Double]) = o match {
     |   case Some(d) => println(s"Value is $d")
     |   case None => println("No value")
     | }
test: (o: Option[Double])Unit

scala> test(data)
Value is 3.3

scala> test(noData)
No value
~~~~~~~~

A value `data` is initialized with `Some(3.3)`, which the compiler infers is of type `Some[Double]`. `noData`, on the other hand, is declared as a `None`.

The `test` function matches against an `Option[Double]` to see whether it contains data or not.

`Option` is often used as a return type:

~~~~~~~~
def getUserDetails(id: Int): Option[User] = {
  if (has_the_user)
    Some(theUser)
  else
    None
}
~~~~~~~~

Using `Option` as the return type does two things for us. First, it makes it apparent that there may not be a value returned, and second, the caller has to deal with this, getting the value out of the `Option` in order to use it.

So how do we get a value out of an `Option`?

~~~~~~~~
scala> val v = data.get
v: Double = 3.3

scala> val vv = noData.get
java.util.NoSuchElementException: None.get
  at scala.None$.get(Option.scala:347)
  ... 33 elided
~~~~~~~~

The `get` method returns the value inside a `Some`, but if you call it on a `None` it throws an exception. The choice of exception is interesting, and emphasises that Scala thinks of an `Option` as a collection. If you don't want an exception, you can check whether your `Option` contains a value:

~~~~~~~~
scala> data.isDefined
res9: Boolean = true

scala> noData.isDefined
res10: Boolean = false
~~~~~~~~

And it may be that you want to provide a default value if the `Option` contains `None`. You can do this using `getOrElse`.

~~~~~~~~
scala> val v2 = noData.getOrElse(0.0)
v2: Double = 0.0
~~~~~~~~

### Handling Options Functionally
Pattern matching works very well with `Option`, but it is also possible to use them in a more functional way.

An `Option` can be thought of as a tiny container that can only hold one object. Since it is a container, we can use the same operations that we use with `List`, such as `map`, `filter` and `foreach`. Here are a few examples to show how they work

~~~~~~~~
scala> data foreach println
3.3

scala> noData foreach println

scala> scala> val square = (n: Double) => n*n
square: Double => Double = <function1>

scala> val m = Option(3.0)
m: Option[Double] = Some(3.0)

scala> m match {
     | case Some(d) => square(d)
     | case None    => 0.0
     | }
res23: Double = 9.0

scala> m map square getOrElse 0.0
res24: Double = 9.0
~~~~~~~~

`foreach` does just what you'd expect, iterating over every element in a container, performing an operation on each element. Of course, in this case there's only one element when the `Option` is a `Some`, and no elements when it is a `None`.

Next we define a simple function to calculate a square, and an `Option`. The `match` returns the square if there's a value, and zero if there isn't.

This can be written more concisely using `map` and `getOrElse`. The `map` takes the value out of the `Option` and squares it, if there is a value. If there isn't a value, `getOrElse` returns zero.

Note how this is created as `Option(3.0)` instead of `Some(3.0)`. If declared as `Some`, the `match` won't compile because `Some` can't match `None`, but `Option` is the superclass and so can match both.

There's a lot more to using `Option`, as we'll see when we talk about functional operations on collections.

## Handling Exceptions
Exceptions are a feature supported by many languages, and they have been around for a long time[^2].

[^2]: They first appeared in PL/1 in the mid-1960s

An exception is an object that describes an exceptional condition, usually containing fields such as a message, a stack trace, and possibly a reference to a previous exception.

They have many advantages, chief among which are that (a) the exception doesn't have to be handled where it is created, and (b) exceptions cannot be ignored, and if not handled will lead to thread or program termination.

In C++, Java and C#, exceptions are created using the `throw` keyword. When an exception is created, execution stops and the call stack is searched, from the current level back up to the root, for a handler. Exceptions are handled using the `try...catch...finally` construct.

T> Java developers, note that Scala does not have checked exceptions.

T> Java developers, also note that you can use Java-style exception handling in Scala. In this chapter I am describing a more functional style of handling exceptions.

### What's Wrong With Exceptions?
The main objection to using exceptions, from a functional programming point of view, is that they are a side-effect. If a function might throw an exception, then you have no guarantee that it will return, and this violates the fundamental principles that we've discussed.

From a more practical point of view, the idea of an exception as an object that describes an error is tied in with the mechanism used to handle it. In JVM languages, an unhandled exception will pass up the call stack, and if it has not been handled by the time it gets to the top, the thread is terminated. This can be a problem if, for instance, your code is executing on a thread from a threadpool or even remotely on another JVM, when you may never find out what happened.

### Functional Exception Handling
In order to be properly functional, code should always return a value. If there is a chance that there may be an exception, then the code should be able to return either the result, or an exception object. Doing this means that it doesn't matter where the code executes, it has predictable behaviour, and it lets the caller decide how and when to handle the error.

It would be possible to do this using `Option`, returning a `Some` when an operation has worked, and `None` if it failed, but the problem here is that you wouldn't have any idea what had gone wrong. Here's how you might use it:

~~~~~~~~
scala> def toInteger(s: String): Option[Int] = 
     |   try {
     |     val n = s.toInt
     |     Some(n)
     |   }
     |   catch {
     |     case ex: NumberFormatException => None
     |   }
toInteger: (s: String)Option[Int]

scala> toInteger("123")
res30: Option[Int] = Some(123)

scala> toInteger("12x")
res31: Option[Int] = None
~~~~~~~~

The `String` class has a `toInt` method that tries to convert the string to an `Int`, throwing a `NumberFormatException` if it can't. So, if the conversion succeeds we return a `Some[Int]`, and if it doesn't, we return `None`. 

The function also illustrates how to code up a traditional `try...catch` construct in Scala. The `try` is the same, but there is only one `catch` block, which contains a `case` for each type of exception you want to handle.

> The fact that exceptions are handled by something that looks like a `match` is not a coincidence. The content of a `catch` is a *partial function*, which defines a map of possible values to expressions.

**Note: insert forward or back ref to where these are discussed

Note that this approach demonstrates the behaviour we want: we always get a result, and we know that the function may fail. The only problem is that we have no information about what went wrong.

### `Try[T]`
Scala provides the `Try[T]` type for representing exceptions. `Try` is similar to `Option` in that a `Try` can have one of two values, but this time the values are `Success[T]` to hold the result, and `Failure` to hold an exception.

We can now rewrite `toInteger` using `Try`:

~~~~~~~~
scala> import scala.util.{Try, Success, Failure}
import scala.util.{Try, Success, Failure}

scala> def toInteger(s: String): Try[Int] =
     | try {
     |   val n = s.toInt
     |   Success(n)
     | }
     | catch {
     |   case ex: NumberFormatException => Failure(ex)
     | }
toInteger: (s: String)scala.util.Try[Int]

scala> toInteger("123")
res32: Option[Int] = Success(123)

scala> toInteger2("abc")
res33: scala.util.Try[Int] = Failure(java.lang.NumberFormatException: For input string: "abc")
~~~~~~~~

The only significant difference between using `Option` and `Try` is that the exception is now returned as a `Failure`.

### Working With `Try[T]`

Just like `Option[T]`, `Try[T]` can be thought of as a small container, and can be used with operations like `map`, `flatMap` and `filter`.
 