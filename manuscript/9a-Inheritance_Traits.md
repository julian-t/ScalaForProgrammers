# Inheritance and Traits
If you have met inheritance in other OO languages such as Java or C#, then the inheritance model used by Scala will be familiar. These languages also support the idea of *interfaces*, which in Scala have been replaced by *traits*.

I'm assuming that readers have a basic understanding of OO, including the ideas of inheritance, abstract types and interfaces. If that isn't the case, read the following section and follow up on some of the references.

### A Brief Overview of Inheritance
This section provides a brief discussion of inheritance; feel free to skip over it if you are comfortable with OO programming. If you want more detail, there has been a lot written on the subject! See [Head First OO Analysis and Design](http://shop.oreilly.com/product/9780596008673.do)

In programming languags, inheritance gives us a way to express the "is a" relationship. For example, a `Car` is a `Vehicle`, a `SavingsAccount` is an `Account`, and a `Manager` is an `Employee`.

T> The parent and child classes will often be called the *superclass* and *subclass*, or *base class* and *derived class* if you come from C++.

Inheritance tells the compiler how classes are related: if a `Car` is a `Vehicle`, then a function that requires a `Car` will accept a `Vehicle`. This principle of *substitutability* lets developers write software that models the world, and helps make it extensible.

There are a couple of important points to note when designing inheritance relationships. If a `Car` is a `Vehicle`, then it must do everything that a `Vehicle` can do. It is allowed to add new functionality and redefine how existing functionality works, but it is not allowed to remove functionality... because if it does, it isn't really a `Vehicle`.

The second point is bear in mind is that you should only use inheritance where you really do want an "is a" relationship. For example, a stack can be implemented using a linked list, but you probably don't want to say that a stack is a linked list, because you can add items to both ends of a list as well as insert them in the middle. What you probably want to do is to implement the stack *using* a linked list... but that is different to being one, and doesn't imply anything extra to the user.

## Inheritance
-- inheritance syntax
Because it runs on the JVM, Scala follows the Java model, with classes inheriting from a single superclass. And like in Java, the relationship is expressed using `extends`:

~~~~~~~~
scala> class Employee extends Person
defined class Employee

scala> val e1 = new Employee
e1: Employee = Employee@2f333739

scala> val p1: Person = e1
p1: Person = Employee@2f333739
~~~~~~~~

As you'd expect, since a, `Employee` is a `Person`, you can get a `Person` reference to an `Employee` object.

-- calling superclass constructors

-- overriding methods

-- overriding def and val

### Abstract Classes and Methods

## Traits
-- traits differ from interfaces in several ways

* traits can contain default implementations of methods
* they can be added to objects at creation time
* they can require that classes using them implement particular behavior
* they solve the "diamond inheritance" problem

### Composing Traits

### Adding Traits at Object Creation Time
