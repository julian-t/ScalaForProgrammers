# Evaluation

## def and val

By now you're used to using `def` and `val`, and the implication has been that you use `def` for defining functions, and `val` for declaring values. This, while often the case, is not the best way to look at things.

In Scala, as in other functional languages, we tend to think at a higher level of abstraction, focussing on what we want to do rather than how it is done or implemented.

When thinking about `def` and `val`, it is more useful to think in terms of evaluation than in terms of what we are defining. Put at its simplest, when we define a function using `def`, we are defining something that will be evaluated later, possibly when we pass it arguments:

~~~~~~~~
scala> def square(n: Int): Int = n*n
square: (n: Int)Int

scala> square(3)
res0: Int = 9
~~~~~~~~

We define something called `square` and then evaluate it later, passing in an argument. But how about this?

~~~~~~~~
scala> val n = 3
n: Int = 3

scala> val m = 4
m: Int = 4

scala> def sum1 = n + m
sum1: Int

scala> val sum2 = n + m
sum2: Int = 7

scala> sum1
res4: Int = 7

scala> sum2
res5: Int = 7
~~~~~~~~

If a function doesn't take any arguments, then we don't have to provide spurious parentheses when we define or call it. So it looks like `sum1` is a function. `sum2`, defined as a `val`, looks exactly the same, and when we evaluate the two we get the same answer.

But take a close look at what the compiler tells you about `sum1` and `sum2`. The line `sum1: Int` tells you that `sum1` is an `Int` but it doesn't have a value. On the other hand, `sum2: Int = 7` tells you that `sum2` is also an `Int`, but that it has the value 7. It has been evaluated at the point of declaration, but `sum1` has not - it is evaluated every time it is used, which in this case is clearly a waste of effort. 

This distinction often shows up when creating classes. Look at this simple `Person` class:

~~~~~~~~
class Person(first: String, last: String) {
 val name = first + " " + last
}
~~~~~~~~

The strings `first` and `last` are parameters to the constructor but don't define fields because they are not annotated with `val`. They are used to construct `name`, which is now a field. However, we could change the class definition, replacing `val` with `def`:

~~~~~~~~
class Person(first: String, last: String) {
 def name = first + " " + last
}
~~~~~~~~

This can be used in exactly the same way, the difference in the two approaches being that the `val` is initialized at the time the `Person` instance is created, while the `def` is evaluated every time it is used.

Ask yourself whether `name` is a field or a method. It turns out that this isn't really the right question. Thinking at a more abstract level, does it really matter whether it is implemented as a field or a method? 

The question is rather *when* we want it to be evaluated.

## Lazy Evaluation

Scala gives us another take on evaluation, and one that is commonly found in functional languages: *lazy evaluation*.

Just about all languages support eager evaluation, when expressions are evaluated as soon as possible. For example, in the following fragment, the addition is done in order to assign a value to `n`:

~~~~~~~~
scala> val n = 3 + 4
n: Int = 7

scala> n
res0: n = 7
~~~~~~~~

Evaluating `n` gives the answer we expect. If we now declare another value but add the `lazy` keyword, we get different behavior:

~~~~~~~~
scala> lazy val q = 3 + 4
q: Int = <lazy>

scala> q
res1: Int = 7
~~~~~~~~

The value of `q` has not been calculated straight away; instead, the expression `3+4` has been stored, and is evaluated the first time `q` is used. After this, the value is fixed and does not need to be evaluated again.

There are, as with many things, advantages and disadvantages to lazy evaluation. The primary advantage is that, if an expression is complex but doesn't get evaluated, then you don't pay the cost. The main disadvantage is that you don't necessarily know when an expression is going to be evaluated, and it can be the case that a slight change in code structure can radically alter the execution profile, with complex expressions being lazily evaluated at a different time.

## Pass-by-name and Pass-by-value

Readers who know C++, C# or Java will be familiar with the terms *pass-by-value* and *pass-by-reference*. Scala adds another one, pass-by-name, which is related to lazy evaluation.

When you pass an argument to a function by value, the function receives the value of the argument. This can be clearly seen in the following example:

~~~~~~~~
scala> def printDates(d: java.util.Date) = {
     | println(d)
     | Thread.sleep(2000)
     | println(d)
     | }
printDates: (d: java.util.Date)Unit

scala> printDates(new java.util.Date)
Thu Sep 29 09:36:58 BST 2016
Thu Sep 29 09:36:58 BST 2016
~~~~~~~~

The `printDates` function takes a `Date` and prints it twice, with a two second delay in between the two calls to `println`. Giving it `new java.util.Date` as its argument causes the expression to be evaluated at the point of call; the reference to this object is passed to the function, and so we get the same value printed twice.

Now make one minor change to the function signature, and run it again:

~~~~~~~~
def printDates2(d: => java.util.Date) = {
     | println(d)
     | Thread.sleep(2000)
     | println(d)
     | }
printDates2: (d: => java.util.Date)Unit

scala> printDates2(new java.util.Date)
Thu Sep 29 09:43:05 BST 2016
Thu Sep 29 09:43:07 BST 2016
~~~~~~~~

Specifying the argument as `d: => java.util.Date` shows that we want to use *pass-by-name*, and says that `d` must be an expression that will evaluate to a `java.util.Date`. The expression `new java.util.Date` is passed to the function, and is then evaluated twice, resulting in a new `Date` object each time. And you can see that the times printed differ by two seconds.

Pass-by-value is a powerful tool in functional programming, and when combined with Scala's ability to call functions as if they were keywords, enables the creation of powerful DSLs.

## Summary

We have three ways to define when an expression will be evaluated

   * `val` uses eager evaluation, the expression being evaluated at the time the `val` is declared 
   * `lazy val` uses lazy evaluation, only evaluating the expression the first time it is used
   * `def` causes the expression to be re-evaluated each time it is used