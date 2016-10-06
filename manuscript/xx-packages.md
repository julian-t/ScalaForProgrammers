# Packages, Imports and the Classpath

Much of what's in this section will be familiar to Java programmers, although there are a few things that are particular to Scala.

## Packages

## Imports

## The Classpath

How do the `scala` and `scalac` commands know where to find compiled Scala code? The answer lies with something called the *classpath*, which Scala has inherited from Java.

The classpath is a list of directories and/or JAR files which will be searched when Scala is trying to find classes. You typically provide Scala with the classpath in one of two ways:

* by creating an environment variable called CLASSPATH
* by providing the `-classpath` option to `scala` and `scalac`

-- set classpath env in Windows and Linux

-- example of using it

-- provide arg to scalac

T> If the classpath isn't set, Scala will default to looking in the current directory.

## JAR Files
