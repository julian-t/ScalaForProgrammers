# Functions

You tried writing an executing a function back in Chapter 1. Let's revisit that, and try another couple of functions:

~~~~~~~~
scala> def square(n: Int): Int = n*n
square: (Int)Int

scala>square(3)
res0: Int = 9

scala> def add(a: Int, b: Int): Int = a+b
add: (Int, Int)Int

scala> add(3, 4)
res1: Int = 7

scala> def getThree = 3
getThree: ():Int

scala> getThree
res2: Int = 3

scala> def hello = println("hello!")
hello: ()Unit

scala> hello
hello!
~~~~~~~~

Functions take a comma-separated list of arguments in parentheses, each of which has a name and a type.

You can see how the signatures of the functions represent their arguments: `(Int, Int)Int` is a function that takes two `Int` arguments and returns an `Int`. The last function is a little more interesting. `hello` is a function that takes no arguments and doesn't return anything. Scala doesn't require you to type braces of any sort that are not needed, and since this function takes no arguments, you don't have to put declare it as `hello()`. You can, and it will do no harm, but you don't have to. Likewise, when you call it, since there are no arguments you don't have to use parentheses there, either.

A> If you define a no-argument function without parentheses, then you must call it without parentheses as well. If you define it with empty parentheses, then you can call it with or without those empty parentheses, as you wish.

Note the return type here: `Unit`. This is the return type of a function that has nothing to return. This function calls `println`, which doesn't return a value, and so it's return type is `Unit`.

## What About The Return Type?

You might have noticed that for some of the functions you just created you gave a return type, but for some you didn't. Type inference means that - with one exception, which we'll get to when we talk about functional programming - the compiler can normally infer what the return type of a function ought to be.

However, it is often thought to be good practice to give the return type for two reasons. The first is documentation, where a declared return type means that someone using your code doesn't have to work their way through the function to do their own type inference. And the second is to guard against surprises. The compiler deduces what the return type ought to be, but what if it isn't what you thought? This can definitely happen if you edit the code and change an important detail. And so it is a good idea to get into the habit of giving your functions explicit return types.

T> If a function doesn't return anything, you can miss out the `=`. This implies that the function is of type `Unit`, but a lot of people don't use this.

## More Complex Functions

The functions you've written so far have simple, one expression bodies. Let's try a couple that have more complex code:

~~~~~~~~
scala> def sayHello(name: String): String = {
     |   val s = "Hello, " + name + "!"
     |   s
     |}
sayHello: (name: String)String

scala>sayHello("fred")
res1: String = Hello, fred!

scala> def applyTax(value: Double, rate: Double): Double = {
     |   val taxPercent = rate / 100.0
     |   value + (value * taxPercent)
     |}
applyTax: (value: Double, rate: Double)Double

scala> applyTax(10.0, 20.0)
res2: Double = 12.0

scala>
~~~~~~~~

When you want to have more than one expression in a function body, and want to have them on separate lines, put them in a block enclosed in curly braces.

Remembering that everything is an expression in Scala, when you have more than one expression in a block, the result of the block is the result of the last expression that was evaluated. So in our sayHello function, the last expression is `s`, which (not surprisingly) evaluates to itself.

If you omit that and just leave the `val s =` line, you'll get a compiler error. Can you figure out why? See [1] in the "Hints and Answers" at the end of this chapter.

The second function calculates tax, given an amount and a tax rate in percent. Simply calculating the amount (and not assigning it to a val) works fine as a return value.

## Arguments

Function arguments will often have a sensible default value. Examples might include conversion from a `String` to an `Int`, where the number base, more often than not, is 10, or a tax rate that would default to the base rate. Try it out:

~~~~~~~~
scala> def applyTax2(value: Double, rate: Double = 20.0): Double = {
     |   val taxPercent = rate / 100.0
     |   value + (value * taxPercent)
     |}
applyTax2: (value: Double, rate: Double)Double

scala>applyTax2(10.0)
res3: Double = 12.0

scala>
~~~~~~~~

There's a little catch here: you can have as many default arguments as you like, but they all have to be at the right-hand end of the list, and once you've taken one as a default, all the ones following it must be defaulted as well.

Now suppose you want to add some numbers. You could define `add2` with two arguments, add3 with three and so on, but this obviously doesn't scale. Here's how you'd do it:

~~~~~~~~
scala> def add(nums: Int*): Int = {
     |   var total = 0
     |   for(n <- nums)
     |     total += n
     |   total
     | }
add(nums: Int*)Int

scala> add(2, 3, 4, 5, 6)
res8: Int = 20

scala> add()
res9: Int = 0

scala>
~~~~~~~~

Notice the `Int*` in the argument list. The `*` tells Scala that the function can take zero or more `Int` values, and these will be packed into a collection and delivered to the function. We will talk about loops in the next chapter, but for now it ought to be reasonably intuitive that `for(n <- nums)` means "take each value in nums and call it n".

You can also see that, since total had a default value of zero, this works when there are no arguments, and thus nothing in the collection to loop over.

### Hints and Answers

[1] The type mismatch error says that it found Unit and wanted String. This is because an expression declaring a val evaluates to Unit, and not to the thing you're declaring. This also shows why it is a good idea to declare the return type: you said it should be a String, and compiler told you that it wasn't.