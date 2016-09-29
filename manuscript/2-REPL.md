# Hello, REPL

Now that you're set up, let's start investigating Scala. We'll do this using an interactive environment called the REPL, which stands for Read-Execute-Print-Loop. Lots of languages—especially interpreted and dynamic languages—have a REPL, including Lisp, Python and Ruby.

The idea of the REPL is simple: it reads an expression, evaluates it, prints the result, and then loops to wait for the next expression. Let's see how it works. Start a REPL by typing scala at a command prompt:

~~~~~~~~
17:43 ~ $ scala
Welcome to Scala version 2.11.6 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_40).
Type in expressions to have them evaluated.
Type :help for more information.

scala>
~~~~~~~~

To exit from the REPL, type `:quit`:

~~~~~~~~
scala> :quit
~~~~~~~~

## Evaluating Expressions

Start the REPL again and type a simple expression at the prompt, such as 2+2. You should see this:

~~~~~~~~
scala> 2+2
res0: Int = 4
~~~~~~~~

Try typing some more expressions involving numbers and strings:

~~~~~~~~
scala> 3.2+2.7
res1: Double = 5.9

scala> "abc" + "def"
res2: String = abcdef

scala> "abc
<console>:1: error: unclosed string literal
       "abc
       ^

scala>
~~~~~~~~

Let's analyze what's going on here. The operations should be obvious—addition of integers, and concatentation of strings, which are delimited by double quotes. Unlike some languages (such as Python and Perl), in Scala strings have to be delimited by double quotes, and single characters by single quotes. You can also see that you don't have to put spaces round operators.

The last example shows you a typical error. The string literal "abc" should be enclosed in double quotes, but you pressed Enter before typing the second one. The compiler tells you what's wrong, and also where the error occurred. Scala's error messages are frequently far more helpful than those you get in some other languages. As you try these examples, it is always a good idea to experiment and see what the compiler says when you make errors.

The expressions that you typed, such as 2+2, are evaluated by the REPL, and their values printed. The REPL figures out the type of the expression, and tells you that as well as the value. It also wants to give the result a name, and since we haven't provided one, it assigns one of its own, starting with res0 and incrementing the number by 1 for each new result. Now try this:

~~~~~~~~
scala> 2
res3: Int = 2
~~~~~~~~

Although it may seem a little strange, 2 is an expression that evaluates to… 2.

Scala doesn't have statements, and everything in Scala is an expression that has a value. We'll see the consequences of this important principle as we learn more about the language.

So what happens if you type the name that the REPL gave to one of your earlier results?

~~~~~~~~
scala> res3
res4: Int = 2
~~~~~~~~

Make sure you understand what's going on here: everything is evaluated as an expression, and the result is given a new name in the REPL.

You might be wondering whether you can give your own name to the result of evaluating an expression, and of course, you can. Try typing some more expressions:

~~~~~~~~
scala> val n = 2 * 3
n: Int = 6

scala> val m = 3.3
m: Double = 3.3

scala> val s = "abc"
s: String = abc
~~~~~~~~

You've now given the name `n` to the result of this expression. The `val` keyword tells Scala that you're defining a new value. Note that I use the term value and not variable. This is deliberate, because values can't be changed once they've been defined, and so calling them variables (while common) seems lazy.

You'll find that parentheses work in the way you'd expect when constructing arithmetic expressions. Try evaluating various kinds of expressions, and use the values you've defined earlier.

## Functions

Now let's execute a function:

~~~~~~~~
scala> println("Hello world")
Hello world

scala>
~~~~~~~~

In Scala, like in many other languages, you call a function by giving its name and the argument list in parentheses. The println function does what you'd expect, and prints its argument. We'll see later on how to do formatted output, but for now, experiment with outputting strings and numeric expressions.

(An aside here for those who might be used to Python - in Scala, strings are delimited by double quotes, and single characters by single quotes. The two are not equivalent at all)

Note what the REPL prints here: it doesn't tell us what was returned by the evaluation of the expression. Instead, it prints the string. "But wait," you say, "I thought you said that everything is an expression."

Indeed it is, but `println` doesn't evaluate to anything useful, so the REPL doesn't bother to tell us. Precisely what it returns and why, we'll come to when we discuss types.

To finish off this chapter, let's define a simple function of our own. Don't worry about the details of the syntax at this point - we have a whole chapter about this coming up.

~~~~~~~~
scala> def square(n: Int) = n*n
square: (n: Int)Int

scala>
~~~~~~~~

Here, `def` introduces the definition of the function square. The function takes one parameter of type `Int`, and the body of the function comes after the equals.

As you might have guessed  by now, the colon syntax is how Scala provides type information. `n: Int` means "n is of type Int", and `square: (n: Int)Int` means "square is a function that takes and Int, and returns an Int".

### Saving and Loading
It is possible to save what you've done in the REPL in a file, and then load it in a later session and carry on from where you left off.

As you might expect, the `:save` command saves the commands you've executed in a file, and the `:load` command reads a file and executes the expressions it contains.

Try typing a couple of definitions into a fresh REPL:

~~~~~~~~
scala> val n = 3
n: Int = 3

scala> def square(n: Int): Int = n*n
square: (n: Int)Int

scala> println("Save everything!")
Save everything!

scala> :save session.scala
~~~~~~~~

The file should contain the following:

~~~~~~~~
val n = 3
def square(n: Int): Int = n*n
println("Save everything!")
~~~~~~~~

Now start a new REPL and load the file. You should see this:

~~~~~~~~
scala> :load session.scala
Loading session.scala...
n: Int = 3
square: (n: Int)Int
"Save everything!
~~~~~~~~

The REPL loads and executes the commands, and you see the result of the executions. You can now carry on to use `n` and execute `square`, and carry on from where you left off.

A> This is a common way of working in functional languages, where you define small functions and then build them up into larger structures. Being able to save your work in the REPL obviously helps.

## Exercises
1. Use the `:help` command to see what other commands you can use in the REPL.
1. Define a `cube` function and test it.
2. Save the `cube` function and its tests in a file, then load it into a new REPL.

