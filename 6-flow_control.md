# Flow Control

You can now write functions, so this is a good place to look at the flow control constructs that Scala gives you.

## Decisions with if

Every language has an `if`: here's how Scala's works:

~~~~~~~~
scala> if (n > 0) 3 else 4
res0: Int = 3

scala> if (n >= 3) true else false
res1: Boolean = true

scala> if (n >= 3) true else "false"
res3: Any = true
~~~~~~~~

If you've programmed in other languages such as Java, you'll notice something a bit unusual about Scala's `if`: it returns a result. As I said in Chapter 1, everything in Scala is an expression, and an if is no exception.

It's quite simple: we know that only one of the branches of an `if` is going to be executed, so the value of the `if` expression is the value of whichever of its branches gets evaluated. In the first example, it's an `Int`, and the second, a `Boolean`.

What's going on in the last example? Here, the two branches returned different types, a `Boolean` and a `String`. The compiler makes sense of this by trying to find a type that can represent both of them, and it finds `Any`. Just what `Any` is, we'll discuss when we get to object-oriented programming. For the minute, bear in mind that if you find an expression evaluates to `Any`, you've possibly got a mixture of types that you might not want.

You can, of course split `if` expressions over more than one line, and have more than one branch. Try typing the `if (n >=3)` example on more than one line:

~~~~~~~~
scala> if (n >= 3)
     | true
res1: AnyVal = ()
~~~~~~~~

Oh dear. The vertical bar is a secondary prompt, showing that the REPL recognizes that you haven't finished an expression. The trouble is that typing `true` *could* make it a complete expression, and so that's what the REPL assumes. The result you get isn't helpful (and we'll explain what `AnyVal` is in a later chapter)

One way to fix this is to use paste mode:

~~~~~~~~
scala> :paste
// Entering paste mode (ctrl-D to finish)

if (n < 3)
 1
else if (n == 3)
 2
else
 0

// Exiting paste mode, now interpreting.

res3: Int = 2
~~~~~~~~

Input is usually processed line by line, but when you type `:paste`, nothing is interpreted until you use Ctrl-D to terminate input. This lets you input multi-line constructs without the REPL jumping in and interpreting them too early.

## Boolean Expressions

The expression is parentheses is a `Boolean` expression, and uses the Boolean operators (`==` and `!=` for equality, along with `>`, `>=`, `<`, `<=` and `!` (for logical NOT)). You can combine expressions with the `&&` (logical AND) and `||` (logical OR) operators. Try this out:

~~~~~~~~
scala> val m = 3
m: Int = 3

scala> val p = -1
p: Int = -1

scala> :paste
if (p > 0 && m < 5)
  true
else
  false

// Exiting paste mode, now interpreting.

res3: Boolean = false
~~~~~~~~

The `&&` builds a Boolean expression, and since `p` is less that zero, the result will be false. Something that isn't immediately evident from this code, but which may become apparent in other examples, is short-circuit evaluation. When building a Boolean expression using `&&` and `||`, evaluation stops as soon as it is obvious what the result is. Here, `p` is less than zero, so the result can never be true, and at this point evaluation stops, and the second clause (`m < 5`) is not evaluated. That makes little difference here, but might if the second clause calls a function that does something.

### Using Blocks with Functions

We know that you can make compound expressions by enclosing them in curly brackets, so let's try that with `if`:

~~~~~~~~
scala> :paste
// Entering paste mode (ctrl-D to finish)

if (n >= 3) {
  println("It's true")
  true
}
else {
  println("It's false")
  false
}

// Exiting paste mode, now interpreting

It's true
res5: Boolean = true

scala>
~~~~~~~~

Once again, you need to use paste mode here, so that the compiler doesn't think the expression finishes at the first closing curly bracket.

## Looping

Scala supports several looping constructs (`while`, `do..while` and `for`) but you'll find that you may not use them in the way (or as often as) you expect.

First, let's look at the `while` loop:

~~~~~~~~
scala> var i = 5
i: Int = 5

scala> while (i > 0) {
     |   println("Going...")
     |   i -= 1
     | }
Going...
Going...
Going...
Going...

scala> 
~~~~~~~~

Here, we've used a `var` as the loop counter, changing its value each time through the loop. But we've already said that we don't like using `var`, preferring immutable values, so what could we do instead? There are various ways to handle this - for example, recursion - and we'll see how to use them later. For now,  note that we can normally find a better way to express what we want to do.

There's also a `do..while` loop, which works in a very similar way:

~~~~~~~~
scala> i = 5
i: Int = 5

scala> do {
     |   println("Going...")
     |   i -= 1
     | } while(i > 0)
Going...
Going...
Going...
Going...
Going...

scala>
~~~~~~~~

The only difference between these two, as you probably know, is when the check is done. The `while` loop is an __entry condition__ loop, checking its expression on entry, but `do..while` is an __exit condition__ loop. This means that the body of the `do..while` will always be executed once.

## The for Loop

The third looping construct we'll look at is the `for` loop. Although this is similiar to loops with the same name in other languages, Scala's for goes way beyond what they can do.

### Internal and External Iteration

I mentioned at the start of this section that loops aren't used as often as you might think. I'll cover the details in the chapter on Functional Programming (***reference***) but for now, I'll briefly discuss the difference between internal and external iteration. __External iteration__ is where you write explicit loops, like you've been doing so far in this section. __Internal iteration__ is where you get some code to do it for you. Compare the following two pieces of code:

~~~~~~~~
scala> scala> for(n <- 1 to 5)
     | println(n)
1
2
3
4
5

scala> (1 to 5).foreach(println)
1
2
3
4
5

scala>
~~~~~~~~

In the first one, you're looping explicitly, telling Scala to assign each of the values from the `Range` to `n`, and then print it. This is external iteration, because you're operating on the `Range` object from outside. In contract, in the second example you're telling the `Range` object itself to do something with each of its members, using `foreach`. This is internal iteration, where an object is doing the iteration for you.

The difference between these may seem minor, but it has profound consequences. Code becomes more compact and declarative in nature: if you know SQL, there are similarities here. When composing a SQL statement, such as a SELECT, you don't tell the database how to perform the operation. You tell it the result you want ("select name, age from customers") and the database engine figures out how to set up and execute the query. The same is true here: internal iteration leaves the door open for the compiler to decide how to do the job.

A second consequence is that a loop only processes items one at a time. A construct such as `foreach` might be able to process many items in parallel: again, you're saying what you want done rather than how to do it.

## Switching with match

Many languages have a multi-way decision statement: C, C++, Java and C# have `switch`/`case`, Ruby and Visual Basic have `case`/`when`. All these can take the place of a chain of `if..else` statements, and are often more efficient.

Scala has `match`, although as we'll see later, it can do a lot more than just act as a switch. Let's see what match can do:

~~~~~~~~
scala> val n = 3
n: Int = 3

scala> n match {
     |   case 1 => println("It's one")
     |   case 2 => println("It's two")
     |   case 3 => println("It's three")
     |   case _ => println("Some other number")
     | }
It's three

scala> val s = "xyz"
s: String = xyz

scala> s match {
     |   case "abc" => println("It's abc")
     |   case "def" => println("It's def")
     | }
scala.MatchError: xyz (of class java.lang.String)

scala> val result = n match {
     |   case 1 => "one"
     |   case 2 => "two"
     |   case 3 => "three"
     |   case _ => "something else"
     | }
result: String = three

scala>  
~~~~~~~~

The first example looks just like a switch. We have taken an `Int` and matched against several possible values, printing out a suitable message. The final case uses the underscore as a wildcard, and so it will match any value. Doing this makes the match complete rather than partial. You can see this is the second example, where there isn't a case for matching the input, and so the result is a `MatchError`.

Since everything in Scala is an expression, it should not be surprising that you can get the result of a `match`, and that is shown in the final example.

### Match and Functions

Match is often used with functions, like this:

~~~~~~~~
scala> def dayName(n: Int): String = n match {
     |   case 1 => "Monday"
     |   case 2 => "Tuesday"
     |   case 3 => "Wednesday"
     |   case 4 => "Thursday"
     |   case 5 => "Friday"
     |   case 6 => "Saturday"
     |   case 7 => "Sunday"
     | }
dayName(n: Int)String

scala> dayName(1)
res0: String = Monday

scala> 
~~~~~~~~

The body of `dayName` is a single expression, a `match` which looks at the value and returns an appropriate string. If you put in a value outside the range 1-7 you will get a `MatchError`: this may not be ideal, and we'll see how to handle errors gracefully in due course.

## Exercises

Write more functions using match