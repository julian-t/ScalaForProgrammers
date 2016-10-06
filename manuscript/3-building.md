# Building Programs

The REPL is an excellent tool - and I'll use it a lot in this book - but you need to be able to build applications and libraries.

This chapter will present an introduction to build tools, starting with the simplest and working up from there.

## scalac and scala

Installing Scala gives you two command line tools: `scalac`, the Scala compiler, and `scala`, which runs Scala code.

I> Anyone who has worked with Java will be familiar with the `javac` and `java` command line tools: these perform the same function for Scala.

The Scala compiler, `scalac`, compiles Scala code into bytecode that can be run on the JVM. Open your favourite editor and type some Scala code into a file, saving it as `Hello.scala`:

~~~~~~~~
// Hello.scala
object HelloApp {
  def main(args: Array[String]) {
    println("Hello!")
  }
}
~~~~~~~~

This defines a singleton type called `HelloApp` that has a `main` method. Having `main` means that this can be run as an application.

I> Unlike other languages such as C++, a Scala application can contain several classes with `main` methods. You choose the one to execute when you run the application.

The `main` method takes an array of strings as its argument, and this will hold the command line arguments. 

You can compile this using `scalac` and run it using `scala`, like this:

~~~~~~~~
$ scalac Hello.scala

$ ls -l
total 24
-rw-r--r--  1 julian  staff   88  6 Oct 12:08 Hello.scala
-rw-r--r--  1 julian  staff  583  6 Oct 12:09 HelloApp$.class
-rw-r--r--  1 julian  staff  571  6 Oct 12:09 HelloApp.class

$ scala HelloApp
Hello!
~~~~~~~~

The output from `scalac` is a pair of `.class` files, which contain the instance and static members of the `HelloApp` class. You execute the application by giving `scala` the name of the *class* you want to execute.

T> Giving `scala` a filename (`HelloApp.class`) rather than a class name (`HelloApp`) is a common mistake.

`scala` will look for the file containing the `HelloApp` class, load it into memory, and execute its `main` method.

-- try renaming main and see the error

-- for details on how Scala locates classes, see Chapter xx (Packages, Imports and the Classpath)

## Meet sbt

## IDEs

Java and C# developers are used to using complex and fully-featured development environments, such as Eclipse and Visual Studio. Three of the most popular Java IDEs have plugins that make them good Scala development tools. These three are:

* IntelliJ, which can be found at <http://www.jetbrains.com/idea>
* NetBeans, which can be found at <http://netbeans.org>
* ScalaIDE, which is based on Eclipse, and which can be found at <http://scala-ide.org>

Each of them offers a free edition, so you can try them all and decide which one you prefer. In addition, all of them support "worksheets", editor panes that function like a graphical REPL window.

I'm not going to talk about IDEs any further, for a couple of reasons. Firstly, we're here to talk about Scala, not about tooling, and so we'll use the simplest tools that do the job. Secondly, things keep changing, and anything you read here about an IDE may be out of date by the time the next version is released. 