# Operators

You'll be familiar with the idea of operators from the other language(s) you know. There are a lot of operators in Scala, but most of the basic ones work in the way you'd expect. These are summarized in the following table, and some points of interest are covered after the break:

|Operator            |Description                      |
|--------------------|---------------------------------|
|+ - * / %           | Arithmetic operators            |
| == != > >= < <=    | Relational operators            |
| && \|\| !          | Logical operators               |
| & \| ^ ~ << >> >>> | Bitwise operators               |
| += -= *= /= %=     | Arithmetic assignment operators |
| >>= <<= &= \|= ^=  | Logical assignment operators    |

The purpose of most of these should be evident, but there are a couple of quick things to note for those who are used to C-family languages
   * there is no ternary `?:` operator, because the `if` expression is an exact equivalent
   * the type of an assignment is `Unit`, so you can't chain assignments (i.e. `a=b=c`)
   * there are no `++` or `--` operators

### Short-Circuit Evaluation

If you've come across the concept of short-circuit evaluation with the logical operators, you can skip this section. Try a couple of logical expressions:

~~~~~~~~
scala> def func1(): Boolean = {
     |   println("func1")
     |   true
     | }
func1: ()Boolean

scala> def func2(): Boolean = {
     |   println("func2")
     |   false
     | }
func2(): Boolean

scala> if (func1 && func2) println("Done!")
func1
func2

scala> if (func2 && func1) println("Done!")
func2
~~~~~~~~

You can see that somehow `func1` hasn't been executed in the second if example.

Many of the languages that derive from C (which include C++, Java, C# and Scala) support the idea of __short-circuit evaluation__. The idea - introduced for efficiency - is that a logical expression is evaluated until there is a result, at which point the rest of the expression is not evaluated at all.

In these examples, the two logical expressions must both be true in order for the `&&` to be satisfied. The first example executes `func1`, which returns true, so it proceeds to evaluate `func2`. Since that returns false the `&&` fails, and so you don't see "Done!". The second example calls `func2` first, but since that returns false the `&&` cannot evaluate to true, and so evaluation stops at this point and `func1` is never called.

### Bitwise Operators

If you don't ever use the bitwise operators (or have used them in Java) you can skip this section.

Scala supports the same set of bitwise operators as Java, and (like in Java) they aren't used very often because we aren't typically working at the bit level.

Scala supports two right-shift operators, `>>` and `>>>`. There is a problem with shifting signed integers: what to do with the sign bit? The first, `>>`, fills the new bit positions with whatever the sign bit is, while `>>>` fills it with zeros.

## Operator Precedence and Associativity

As you'd expect, operators in Scala have well-defined rules for precedence and associativity.
