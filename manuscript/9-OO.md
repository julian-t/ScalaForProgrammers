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

## Adding Members

Members can be added to classes by placing them in braces, like this:

~~~~~~~~
scala> class Person(val first: String, val last: String) {
     |   val name = first + " " + last
     | }
defined class Person


scala> val p2 = new Person1("steve", "james")
p2: Person = Person@14e2e1c3

scala> p2.name
res14: String = steve james
~~~~~~~~

The class now has a field called `name`. Since it is a `val`, it is initialized on object creation and is immutable. This is fine, because `first` and `last` are also immutable.

Q> What would be the effect of changing the definition of `name` from `val` to `def`? See the end of the chapter for the answer.

You can add methods as well as fields to classes. Let's add date of birth (which presumably is immutable) and a function to return the age.

~~~~~~~~
scala> import java.time._
import java.time._

scala> class Person(val first: String, val last: String, 
               val dob: LocalDate) {
     |   val name = first + " " + last
     |
     |   def age = LocalDate.now.getYear - dob.getYear
     | }
defined class Person

scala> val aDate = LocalDate.of(1982, Month.APRIL, 2)
aDate: java.time.LocalDate = 1982-04-02

scala> val p4 = new Person("john", "frances", aDate)
p4: Person = Person@74834afd

scala> p4.age
res19: Int = 34
~~~~~~~~

This code uses the Java 8 date and time library, and makes its members available without qualification by using an `import` statement. See chapter **X** for more details on libraries and imports.

### Defining Your Own Getters and Setters

You saw how to add a field to a class when you declared `name` as a `val`, but what if you want to add a settable field? Using a `var` is one solution, but that isn't satisfactory if you want to add some code to validate the getter or setter.

You can define a pair of getter and setter by providing two `def`s: a getter with the name of the value, say `xxx`, and a setter called `xxx_=`. The `_=` tells the Scala compiler that this is a getter method.

~~~~~~~~
scala> import java.time._
import java.time._

scala> class Person(val first: String, val last: String) {
     |   val name = first + " " + last
     |   
     |   private var privateAge = 0
     |   
     |   def age = privateAge
     |   def age_=(newAge: Int) = {
     |     if (newAge > privateAge) privateAge = newAge
     |   }
     | }
defined class Person
~~~~~~~~


## Object Construction
The parameters that you provide in the class definition define the *primary constructor* for the class. It is possible to add other constructors, should you wish, and these are called *secondary (or auxiliary) constructors*.

Suppose that the Person class keeps track of when each instance is created. You can supply a value when you create an instance, but it would be useful to default it to 'now':

~~~~~~~~
scala> import java.time._
import java.time._

scala> class Person(val first: String, val last: String, 
               val created: LocalDateTime) {
     |   val name = first + " " + last
     |   
     |   def this(first: String, last: String) {
     |     this(first, last, LocalDateTime.now)
     |   }
     | }
defined class Person
~~~~~~~~

A method called `this` defines an auxiliary constructor, and you can see that it uses the procedure syntax, because it doesn't have an `=` before the body.

Secondary constructors must end up calling the primary constructor. The idea here is that the primary constructor defines the set of parameters that are necessary for a well-formed object, so it must be called at some point.

T> If you want to call the primary constructor with a fixed value, then a default argument will be a better solution.

## Exercise

Create an `Address`, add field to `Person`, exercise it

## Hints and Answers

The effect of changing the definition of `name` from `val` to `def` is that it will now be evaluated every time it is used.
