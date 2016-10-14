-# Part 4 - Functional Programming

# Functional Programming

While Scala can be used purely as an object-oriented language, one of its great strengths is its support for *functional programming*.

## What is Functional Programming?

Like object oriented programming, functional programming had been around for a long time before it became widely known. And also like OO, now every language that wants to be taken seriously has to suport at least some degree of functional behaviour.

So what are we talking about when we say "functional programming"? There's no one definition, but most people would agree that it is a style of programming characterized by:

* treating computation as function evaluation
* functions-as-data, and higher-order functions
* the avoidance of mutable state
* recursion
* lazy evaluation

As with any other style of programming, you can do it to a certain extent in any language, but it is made a lot easier if you have language-level support.

### Characteristics of Functional Code
Treating code like mathematical functions has several interesting properties.

Consider a simple mathematical function, such as `y = x * 2`. Its equivalent in Scala would be

~~~~~~~~
def twice(x: Int): Int = x * 2
~~~~~~~~

From the way this is defined, we can assert the following:

* a call to `twice` will always work
* it will always return the same result for the same input
* the result depends solely on the input
* it does not interact with the outside world in any way - it has no side-effects
* everything is immutable

These, in turn, lead to some interesting properties:

* Since we always get the same result for the same input, we could cache results to save having to recompute them
* Always returning the same result, and not interacting with the outside world, means that we can replace a call with its result. In other words, `twice(3)` can be replaced by `6` with no consequences
* those two reasons also mean that such functions are trivial to parallelize: multiple threads can execute the same code with no problem

Two more things are worth mentioning as well. Look at the following piece of code:

~~~~~~~~
val a = twice(3)
val b = twice(4)
~~~~~~~~

The computations of `a` and `b` are completely independent, sharing no state. This means that they can be evaluated in any order, or even in parallel. And then suppose that `b` is never used: it is completely safe for the compiler to remove the entire line, because the evaluation of `twice(4)` does not affect anything else.

A function that behaves like this is called a *pure function*, and they have many advantages. But programs cannot be composed completely of pure functions, because any interaction with the outside world (I/O, changing memory locations) makes a function impure.

A lot of functional programmers structure their code so that computation takes place in a core of pure functions, with impure operations being confined to an outer layer.

### Anonymous Functions
Aka lambdas

Function literals compared with other kinds of literals

### Higher-Order Functions

Functions-as-data

Functions as arguments and return types

### Working with Functions
Currying

compose / andThen

Recursion / tail recursion

partial function evaluation

Partial functions

Lazy evaluation