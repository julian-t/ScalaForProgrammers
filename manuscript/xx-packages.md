# Packages, Imports and the Classpath

Much of what's in this section will be familiar to Java programmers, although there are a few things that are particular to Scala.

## Packages

Packages provide a way to group classes. For example, the classes `Square` and `Circle` might belong to a `geometry` package, while `Account` and `Transaction` might belong to a `bank` package.

Like namespaces in other languages, packages provide an extra level of scoping for types.

Packages can be arranged in a hierarchy, and Scala follows the Java convention for naming packages. You will see names like this:

~~~~~~~~
org.scalatest.Assertions
scala.collection.concurrent.Map
org.w3c.dom.NodeList
~~~~~~~~

Package names consist of identifiers separated by periods, with the names going from most general on the left to most specific on the right.

Package names that start with `org` or `com` reflect the original Java convention, which was to start package names with a reversed domain name. If your domain was `w3c.org`, then you would start your package names with `org.w3c`, and this provided a simple way to ensure uniqueness of packages.

Unlike many languages, Scala package names determine where the compiled code has to live. To illustrate this, let's create an application in a package:

~~~~~~~~
package mycompany.myapp

object HelloApp2 {
    def main(args: Array[String]) {
        println("Hello again!")
    }
}
~~~~~~~~

The `package` declaration is at the start of the file, and so applies to everything within the file. Compile this from the command line using the `-d` option, which tells the compiler where to put the generated files. Specifying `.` as the path means that generated files will be under the current directory.

~~~~~~~~
$ scalac -d . Hello2.scala

$ ls
Hello2.scala mycompany/

$ tree
.
├── Hello2.scala
└── mycompany
    └── myapp
        ├── HelloApp2$.class
        └── HelloApp2.class

2 directories, 3 files
~~~~~~~~

Compiling the code creates two subdirectories containing the class files, and you can see that each component of the package name corresponds to a new subdirectory in the tree.

It is very important to appreciate that the fully-qualified name of a class includes its package. So the name of our example class is `mycompany.myapp.HelloApp2`, and there is no such type as simply `HelloApp2`.

You can see this when you try to run the application:

~~~~~~~~
$ scala HelloApp2
No such file or class on classpath: HelloApp2

$ scala mycompany.myapp.HelloApp2
Hello again!
~~~~~~~~

To summarize, you can put source code wherever you want, but compiled code has to live under a directory structure corresponding to the package name.

## Imports

You can always refer to a class by its fully-qualified name, but that may not be desirable when names get long. The `import` keyword lets you make the names in a package available in the current scope without qualification. For example, you could refer to `org.w3c.dom.NodeList` in full every time you use it, or say `import org.w3c.dom.NodeList` and then refer to it as `NodeList` thereafter.

Obviously if there is more than one `NodeList` in scope then the compiler will complain. 

It's important to understand that `import` isn't doing anything physical - it isn't importing code into your project, or anything like that. What it does is to make the names in a package available for use without full qualification. What you are effectively saying is "if you see something you don't recognize, try putting `org.w3c.dom` in front and see if that resolves it."

If you want to use several types from the same package, there are two schools of thought on how you should do it. The first says that you should import them all separately:

~~~~~~~~
import mypackage.Foo
import mypackage.Bar
import mypackage.Baz
~~~~~~~~

The other way uses a wildcard to make all the names available:

~~~~~~~~
import mypackage._
~~~~~~~~

Which you use is a matter of personal preference: the first way makes it evident what is being imported and from where, but it involves a lot more typing.

### Scoping Imports
If you've used Java, you'll be used to declaring imports at the top of a source file. In Scala, you can use `import` in any block, so the names you import are in scope within that block.

~~~~~~~~
// Imports visible to all code below
import mypackage._

def func(n: Int) = {
  // Bar is visible within this function
  import otherpackage.Bar
  {
    // Bar and everything from stuff visible in this block
    import stuff._
    //...
  }
}

def func2(s: String) = {
  //...
}
~~~~~~~~


### Multiple Imports and Aliases

If you don't want to use a wildcard but also don't like typing multiple `import` lines, Scala gives you an alternative:

~~~~~~~~
import mypackage.{Foo, Bar, Baz}
~~~~~~~~

This, intuitively, means "import `Foo`, `Bar` and `Baz` from `mypackage`".

It may also sometimes be the case that you want to import classes with the same name from different packages, such as `java.util.Date` and `java.sql.Date`. Rather than have to use them fully qualified, you can set up an alias

~~~~~~~~
import java.sql.{Date => SqlDate}
~~~~~~~~

Now you can use `SqlDate` and the compiler will know what you are referring to.

### Default Imports

Although all the Scala library classes belong to packages, you don't have to import everything. For example, a `Map` is a `scala.collection.immutable.Map`, but you don't have to import the package or use its fully qualified name. This is because some things are imported by default, and these are

* `java.lang._`
* `scala._`
* `scala.Predef._`

The most interesting one in this list is `Predef` which, as you might expect, stands for "predefined".

`scala.Predef` is an object that provides definitions that can be used in Scala code without explicit qualification. Among the most important things defined in `Predef` are

* `String` as an alias for `java.lang.String`
* `Map` and `Set` as `scala.collection.immutable.Map` and `scala.collection.immutable.Set` respectively
* the `???` function
* the print functions `print`, `println` and `printf`
* assertions
* conversion functions

## The Classpath

How do the `scala` and `scalac` commands know where to find compiled Scala code? The answer lies with something called the *classpath*, which Scala has inherited from Java.

The classpath is a list of directories and/or JAR files which will be searched when Scala is trying to find classes. You typically provide Scala with the classpath in one of two ways:

* by creating an environment variable called CLASSPATH
* by providing the `-classpath` option to `scala` and `scalac`

-- set classpath env in Windows and Linux

In Linux or OS X, you set the classpath as a list of directory paths separated with colons:

~~~~~~~~
$ CLASSPATH=/projects/first:/testing
~~~~~~~~

Windows is similar, except you use semicolons to separate the paths:

~~~~~~~~
C:\> set CLASSPATH=\projects\first;\testing
~~~~~~~~

-- example of using it

As an alternative to using an environment variable - or to override it - you can provide the classpath as an argument to `scalac` and `scala`.

~~~~~~~~
$ scala -classpath=/path/to/lib Hello
~~~~~~~~

If the classpath isn't set, Scala will default to looking in the current directory. This is why the `HelloApp2` example worked.

Let's look at how we can use the classpath to execute `HelloApp2`. To recap, if you're in the directory that holds `mycompany`, then you can execute the app directly:

~~~~~~~~
$ pwd
/Users/julian/tmp/scala

$ scala mycompany.myapp.HelloApp2
Hello again!
~~~~~~~~

However, if you move somewhere else (such as your home directory) you'll find that the app will no longer run from the new location:

~~~~~~~~
$ cd

$ scala mycompany.myapp.HelloApp2
No such file or class on classpath: mycompany.myapp.HelloApp2
~~~~~~~~

This is because `scala` doesn't know where the `mycompany` directory is. You can tell it by setting the classpath:

~~~~~~~~
$ export CLASSPATH=/Users/julian/tmp/scala
$ scala mycompany.myapp.HelloApp2
Hello again!
~~~~~~~~

As an alternative, you can provide a classpath using an option to `scala` and `scalac`:

~~~~~~~~
$ scala -classpath /Users/julian/tmp/scala mycompany.myapp.HelloApp2
Hello again!
~~~~~~~~


## JAR Files
JAR (Java ARchive) files are widely used as a way to package and distribute libraries and applications, and are useful because code that runs on the JVM tends to consist of a lot of individual class files.

When they invented JAR files, Sun decided to use the existing Zip file format rather than invent their own, and this makes it possible to use many third-party tools to create, manipulate and examine JAR files.

T> Different types of JAR file are used in a number of places in the Java world. For example, WAR (Web ARchive) files are used to package web applications. These work the sae way as all other JAR files, but will imply a particular directory structure and other content.

-- JAR files maintain the directory structure for the files you add to them, and so can be thought of as a directory tree held in a file. This means that you can add JAR files to the classpath, where the JVM will search them for the classes it needs at runtime.

-- the `jar` command

-- the manifest

-- example