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

## Adding Fields

## Object Construction

## Adding Members
