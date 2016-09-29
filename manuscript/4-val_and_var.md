# val and var

In this chapter you're going to meet two important Scala keywords: `val` and `var`. You'll also meet a very important concept, immutability.

You've already met `val` when working in the REPL, where you used it to create named values. I mentioned at that point that values can't be changed once they've been created, and you'll get an error if you try:

~~~~~~~~
scala> val n = 3
n: Int = 3

scala> n = 4
<console>:8: error: reassignment to val
 n = 4
 ^
scala>
~~~~~~~~

As this point, you might be wondering two things: why can't I change a `val`, and what if I need to, for something like a loop counter?

To address the first point: the rather flip answer is that Scala gives you both values and variables, and `val` is the immutable one. Rather more seriously, Scala prefers that you use values - immutable data - where possible, and so you will see `val` used extensively. If you know languages where you can change anything, you might be surprised how seldom you need to modify data items. This even applies to data structures, such as lists and maps, and we'll see this in action in later chapters.

> Immutability is a fundamental idea in functional programming. Once you've defined a value, it doesn't change, and this applies to arrays and lists as well as simple items. If data can't change, then this has several important consequences. Most importantly, if it can't change, then we don't have to worry that it might, and this can lead to simpler and more robust code when desling with concurrency.

And to answer the second point: if you do need a proper variable, one whose value you can change, use var instead of val.

~~~~~~~~
scala> var m = 3
m: Int = 3

scala> m = 4
m: Int = 4

scala>
~~~~~~~~

You may think that var is used a lot, but you'll find that you won't use it as much as you expect. The principle to remember is "use var for things that have to be mutable:, and as we continue to learn Scala, look at how often we need to use them.

Note that if you do want to use a `var`, it is good practice not to let it 'escape' from the context in which you use it. Use it within a function or code block, but don't let it escape so that it can be seen elsewhere.