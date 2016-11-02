# A-Introduction to Functional Theory

A brief intro to some of the theory behind functional programming.

This won't satisfy anyone who has a computer science background, but is intended to provide a lightweight introduction for those who are developers first and theorists second.

This appendix introduces some of the concepts that underly a lot of the functional programming. It isn't necessary to understand these concepts in order to use Scala to write production code, but a basic knowledge will help you to understand how some things work, and more importantly, why things work the way they do.

For a more theoretically complete treatment, I'd highly recommend Bartosz Milewski's [Category Theory for Programmers](https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/) articles. 

## Definitions

(Very simplistic, adapted from a post on r/scala)

Semigroup: a binary associative operator, such as `+` or `*`

**Monoid**: a semigroup with a zero element. `Int` forms at least two monoids, one with `+` as the operator and `0` as the zero element, and the other with `*` and `1`. You can see that the same applies to strings (`+` and the empty string) and lists (`++` and `Nil`)

**Functor**: a data structure you can map over. Arrays and lists are functors, because you can use `map` with them, and so are `Option` and `Try`.

If you have a function such as `toString`, which has the signature `Int => String`, you can convert a single `Int` to a `String`. Using `map` now lets you convert a `List[int]` to a `List[String]` - that's what a functor lets you do.

Functors lift single argument (arity 1) functions into a computational context.

**Applicative**: a data structure you can map over, with some structure that can't be shared

Alternatively, for functors, you have a single operation and a container. For applicatives, you have a container of functions and container of values, and are combining the two.

Haskell example

~~~~~~~~
[(*2), (+1)] <*> [1, 2, 3] = [2, 4, 6, 2, 3, 4]
~~~~~~~~

Here, the `(*2)` function is applied to the list, and then the (+1) is applied, and the results concatenated. Note that the result of one isn't known to the second.

In category theory, applicatives also have a method called `pure`, which returns a container with a single item. So for a list in Haskell, `pure "hi!"` gives you `["hi!"]`, a list of one item.

Another explanation: functors work with arity-1 functions, while applicatives can work with functions with higher arity.

For example, you could have two boxed values, and an applicative could execute `+` on them, taking the values our of the boxes, adding them, and putting them back.

**Monad**: a data structure you can map and flatMap over, having some structure that can be shared. This implies that monadic operations are done sequentially

**Traversable**: a data structure you can apply monadic operations to all elements of and/or fold into a smaller structure