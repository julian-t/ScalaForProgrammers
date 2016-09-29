-# Part 3 - Object-Oriented Programming

# OO Programming
This part of the book is going to show how Scala supports object-oriented programming.

Scala has full support for the OO programming features you already know: classes with fields and methods, public and private access, instance and static members, inheritance and interfaces.

It also has a few features that are particular to Scala, in particular *case classes* and *traits*, and we'll discuss these in some detail.

## Creating Classes and Objects

Here's the simplest class definition:

~~~~~~~~
scala> class Foo
defined class Foo

scala> val f = new Foo
f: Foo = Foo@41beb473
~~~~~~~~

Typing `class Foo` defines a class with no members, and since this is Scala, we don't need to put unnecesary empty braces after the class name.

Instances are created using `new`, just as they are in Java, and this returns a reference to the newly created object. The compiler doesn't know what to print for the value of this object, so you get the output from the inherited `toString` method. You'll find out how to override this when we cover inheritance.

A> Reference types are garbage collected, meaning that you allocate them, and the runtime takes care of reclaiming the memory when they are no longer needed. If you haven't come across this idea before, you can read more about it at **insert reference**

## Adding Fields

Classes typically have fields that hold object state. Here's a simple `Person` class:

~~~~~~~~
scala> class Person(val first: String, val last: String)
defined class Person

scala> val p1 = new Person("fred", "smith")
p1: Person = Person@53bf7094

scala> p1.first
res11: String = fred

scala> p1.last
res13: String = smith
~~~~~~~~

`Person` now takes two parameters, representing the first and last name, and we can access then as fields of the object using the familiar dot notation.

The use of `val` as a qualifier on the parameters tells Scala that we want these to be fields, and so Scala generates an implicit getter method for them.

If you're used to OO programming in other languages, you might be worried about the idea of fields being publicly accessible. That isn't a problem in Scala, though, because fields declares using `val` are (you guessed) immutable, and so there's no problem making them public. You can easily verify this by trying to change a value:

~~~~~~~~
scala> p1.first = "dave"
<console>:9: error: reassignment to val
       p1.first = "dave"
                ^
~~~~~~~~

You can make a constructor parameter a `var` in order to make it mutable, but please don't. We'll see better ways to handle mutable state later.

## Object Construction
Secondary constructors

## Adding Members
