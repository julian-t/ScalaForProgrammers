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

Like other languages that run on the JVM, Scala supports inheritance from a single concrete superclass. Some other languages, in particular C++, allow inheritance from more than one concrete superclass, but JVM languages do not.

This means that if you want to model a `SeaPlane`, you can't simply inherit from both `Plane` and `Boat`. However, Scala has introduced the idea of *traits*, which provide an elegant way to do this.

## Inheritance
Because it runs on the JVM, Scala follows the Java model, with classes inheriting from a single superclass. And like in Java, the relationship is expressed using `extends`:

~~~~~~~~
scala> class Person
defined class Person

scala> class Employee extends Person
defined class Employee

scala> val e1 = new Employee
e1: Employee = Employee@2f333739

scala> val p1: Person = e1
p1: Person = Employee@2f333739
~~~~~~~~

As you'd expect, since a, `Employee` is a `Person`, you can get a `Person` reference to an `Employee` object.

### Calling Superclass Constructors
You will often need to pass values to the constructor of a superclass, and this is a straightforward extension of the way you create simple classes.

~~~~~~~~
scala> class Person(val name: String)
defined class Person

scala> class Employee(name: String) extends Person(name)
defined class Employee
~~~~~~~~

The `extends` clause specifies the superclass constructor, and passes values from the subclass constructor argument list. Note, however, that `Employee` doesn't qualify `name` with `val`, because it isn't going to be a field in `Employee` but is simply an argument that gets passed through to `Person`.

### Overriding Methods
As you'd expect, a subclass can override methods that it inherits from its superclass. Here is a class that override the default implementation of `toString`:

~~~~~~~~
scala> scala> class Person(val name: String) {
     | override def toString = "Name: " + name
     | }
defined class Person

scala> val p1 = new Person("fred")
p1: Person = Name: fred
~~~~~~~~

Now, when you create a `Person`, the compiler will display the result of calling `toString` on the object. The result is a lot more useful than the default!

The `override` keyword is required, so that the compiler knows what you are trying to do. Failing to provide it gives an error, as does putting `override` on a method that isn't overriding anything:

~~~~~~~~
scala> class Person(val name: String) {
     | def toString = "Name: " + name
     | override def fooo = "nothing"
     | }
<console>:8: error: overriding method toString in class Object of type ()String;
 method toString needs `override' modifier
       def toString = "Name: " + name
           ^
<console>:9: error: method fooo overrides nothing
       override def fooo = "nothing"
~~~~~~~~

This is useful information, since typing errors can mean that you may not specify an override at all, or worse, accidentally override the wrong method.

### Overriding `def` and `val`
We've already talked about the difference between declaring something as `def` or `val`: a `val` is evaluated the first time it is encountered, while a `def` is re-evaluated each time it is used. This is why it makes sense to use `def` when defining functions, because the whole point is to evaluate them each time they're called.

Suppose, though, that we have this situation:

~~~~~~~~
scala> class Person(val name: String) {
     | def age = calculateAge()
     | }

scala> class AlwaysYoung(val name: String) extends Person(name) {
     | override val age = 21
     | }
~~~~~~~~

The `Person` class defines `age` as a `def`, which is evaluated each time because people gradually grow older. The `AlwaysYoung` class fixes the age at 21, and so it makes sense to define this as a `val`, since it never changes. This is sensible, because `age` in the superclass can return any value, while the override in the subclass restricts it to one value.

It should also seem reasonable that the opposite is not true. If a class has defined a member as a `val`, then clients will expect a single fixed value. Overriding this with a `def` implies that different values may be returned from different calls, which is breaking the contract. And sure enough, if you try this you will get a compiler error:

~~~~~~~~
scala> class P1(val name: String) {
     | val age = 42
     | }
defined class P1

scala> class N1(name: String) extends P1(name) {
     | override def age = 21
     | }
<console>:9: error: overriding value age in class P1 of type Int;
 method age needs to be a stable, immutable value
       override def age = 21
~~~~~~~~

## Traits
-- traits differ from interfaces in several ways

* traits can contain default implementations of methods
* they can be added to objects at creation time
* they can require that classes using them implement particular behavior
* they solve the "diamond inheritance" problem

Traits cannot have constructor parameters

### Layering Traits

### Adding Traits at Object Creation Time

## Abstract Types
As you might also expect, Scala supports abstract classes and methods, and it has a few additions above and beyond what you may be used to in other languages.

You can create an abstract class by simply adding the `abstract` modifier to a class definition:

~~~~~~~~
scala> abstract class Shape
defined class Shape

scala> val s = new Shape
<console>:8: error: class Shape is abstract; cannot be instantiated
       val s = new Shape
~~~~~~~~

You can, of course, extend abstract classes and override methods that they define:

~~~~~~~~
scala> abstract class Shape(xPos: Int, yPos: Int) {
     |  private var xp = xPos
     |  private var yp = yPos
     |
     |  def x = xp
     |  def y = yp
     |
     |  def move(dx: Int, dy: Int) {
     |    xp += dx
     |    yp += dy
     |  }
     |
     |  // area() is abstract
     |  def area(): Double
     |}
defined class Shape

scala> class Circle(xPos: Int, yPos: Int, val radius: Int) extends
  Shape(xPos, yPos) {
     |
     |  def area(): Double = Math.PI * radius * radius
     |}
defined class Circle

scala> val c1 = new Circle(10, 10, 3)
c1: Circle = Circle@7f86313a

scala> c1.area()
res0: Double = 28.274333882308138
~~~~~~~~

An abstract method is defined without a body, but you don't need to indicate that it is abstract in any way: the absence of a body tells the compiler what it needs to know. Note that you don't need to use `override` when implementing an abstract method.

When would you choose to use an abstract class over a trait, given that they can both be used as superclasses? There are two possible reasons, one reasonably obvious and one less so.

The obvious reason to use an abstract class is if your abstract supertype requires constructor parameters, because you can't specify these for traits.

The second reason is for interoperation with Java, because abstract classes will often be easier to use from Java than traits.

We have just seen an abstract `def`, but you can also create abstract `val` and `var` members as well. The idea of an abstract field may seem strange, but as we've already said, the distinction between `val` and `def` is more concerned with when entities are initialized rather than whether they are fields or methods. Abstract `val` members can be useful in traits, providing a way to initialize `val`s in traits where you would use constructor parameters in classes.

A final thing to note is the idea of an *abstract type*, shown in the following example:

~~~~~~~~
scala> trait Adder {
     |   type T
     |   def transform(value: T): T
     | }
defined trait Adder

scala> class UseStrings extends Adder {
     |   type T = String
     |   def transform(value: T) = value + value
     | }
defined class UseStrings
~~~~~~~~

The trait `Adder` defines an abstract type `T` along with a `transform` method that uses the type. The concrete class `UseStrings` completes the definition of `T`, and then uses it.

You may be wondering when you might use abstract types rather that generic classes. The short answer is that most of the time you can use either to get the same effect, but there are some cases where they can be preferable. You can find a discussion on the topic [here](http://www.artima.com/forums/flat.jsp?forum=106&thread=270195).
