# Getting Set Up

This first lesson gets you set up ready for the rest of the chapters.

In order to write and run Scala code, you need three things: a code editor, the Java SDK (Software Development Kit) and the Scala SDK. You need Java as well as Scala because Java is actually the platform on which Scala runs, although you won't see too much evidence of that here.

You'll be using the command line, and not fancy development environment. This may be a little more painful if you're using Windows rather than a Mac or Linux, but you can still make it work fine.

If you talk to Scala developers, you may hear of something called "Activator". This is a tool that can be used to set up skeleton Java and Scala projects of various types. It is useful, but until you're familiar with how things work, it is best not to use it.

## Windows

Download the Java 8 Standard Edition Development Kit (JDK) from the Oracle web site at <http://www.oracle.com/technetwork/java/javase/downloads/index.html>. Make sure that you download the JDK, and not either the JRE or Server JRE.

After clicking on the download button and accepting the licence agreement, you can download the installer for Windows, choosing the 32- or 64-bit version as appropriate.

Once it's installed, you need to set the `JAVA_HOME` environment variable. Do this in the normal way, via the Control Panel, setting its value to wherever you've installed Java, e.g. `C:\ProgramFiles\Java\jdk1.8.0_40`.

Now download the Scala installer from the main Scala site at <http:scala-lang.org>. At the time of writing, the current version was 2.11, with 2.12 just around the corner. Unlike Java, this comes as a compressed file rather than an installer, so unzip it in a suitable directory. You should now set the `SCALA_HOME` environment variable to point to the root of your Scala installation, and add the `bin` directory from the Scala distribution to your path.

Once you've got Java and Scala set up, open a command prompt. You can test whether your installation has worked by typing the `scala` command, which should show you something like this:

***ToDo***

## Mac OS X

Download the Java 8 Standard Edition Development Kit (JDK) from the Oracle web site at <http://www.oracle.com/technetwork/java/javase/downloads/index.html>. Make sure that you download the JDK, and not either the JRE or Server JRE.

After clicking on the download button and accepting the licence agreement, you can download the .dmg installer for OS X.

Once you've run the installer, make sure that the `JAVA_HOME` environment variable is set to point to

the place where the JDK is installed. This isn't quite as simple on OS X as it is on other systems, but you'll find Java installed in `/Library/Java/JavaVirtualMachines`. Here's what the environment variable is set to on my machine:

~~~~~~~~
10:29 ~ $ echo $JAVA_HOME
/Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home
~~~~~~~~

Download the Scala installer from the main Scala site at <http://scala-lang.org>. Note that Scala can also be installed using tools such as Homebrew, but I find that download a compressed archive is simpler and less trouble prone. At the time of writing, the current version was 2.11, with 2.12 just around the corner. Unlike Java, this comes as a compressed file rather than an installer, so unzip it in a suitable directory.

You'll now need to set the `SCALA_HOME` environment variable to point to the root of your Scala installation, and add the `bin` directory from the Scala distribution to your path.

Once you've got Java and Scala set up, open the Terminal app. You may want to put this in the dock, because you'll be using it a lot. As an alternative to Terminal, you might consider [iTerm](https://www.iterm2.com/), which gives you a very fully-featured terminal indeed. You can test whether your installation has worked by typing the `scala` command, which should show you something

like this:

~~~~~~~~
17:43 ~ $ scala
Welcome to Scala version 2.11.6 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_40).
Type in expressions to have them evaluated.
Type :help for more information.
scala> :quit
17:43 ~ $
~~~~~~~~

Typing `:quit` is how you exit from Scala's interactive mode and get back to the command line. Note that the exact version

numbers that you see for Java and Scala may differ from those shown here, but as long as the `scala` command runs, you're good.

## Linux

Many Linux systems will already have Java installed, but the default version may be 1.7, and you'd be wise to upgrade to 1.8. (Not least because the forthcoming 2.12 version will only run on Java 8). You can check the version in a terminal window; here's what I see on my Linux box:

~~~~~~~~
17:43 ~ $ java -version
java version "1.7.0_79"
OpenJDK Runtime Environment
...
~~~~~~~~

This is running an old version, it is the open source OpenJDK rather than the Oracle version, but more importantly, it is the JRE rather than the full Development Kit.

Make a directory for the new version of Java - mkdir /opt/java && cd /opt/java. Now download the tarball from the Oracle site, and extract it here (`tar zxvf ...`) .

Then `cd` to the Java directory, and tell the system where to find the 'java' command:

~~~~~~~~
$ cd jdk_1.8.0_45
$ update-alternatives --install /usr/bin/java java /opt/java/jdk1.8.0_45/bin/java 100
$ update-alternatives --config java
~~~~~~~~

Repeat this for the 'javac' and 'jar' commands. You should now find that the new version is used by default.

## Editors

Many Scala developers, especially those who come from a Java background, will use an IDE, normally IntelliJ or Eclipse, or less frequently Netbeans. However, while modern IDEs are wonderful things, they can lead to a point-and-click way of programming that relies on code completion and other functions that your servant the IDE performs for you... and you can end up like the lord living in a grand castle, surrounded by servants who tend to his every whim, but who doesn't know where anything actually is or how anything works.

We're not going to do that here, and are going to use a plain old code editor, building and running our code on the command line.

Working with a code editor and command line will give you a firm grounding, and once you have that, by all means move on to IntelliJ or one of the other IDEs.

The choice of code editor is a personal thing: some people swear by vim or emacs, while other people swear at them. Personally, I am a vim user and find emacs hard to get along with, but that's just me.

I would recommend trying some of the popular editors, and see which one takes your fancy. For what we're doing, though, choosing an editor is like choosing a compact car for running errands round town - just about any one will do as long as itis reliable and has four wheels, and it is just a case of picking the one that you feel most comfortable with.

On Windows, [Notepad++](http://notepad-plus-plus.org) is justifiably popular, easy to use, and free.

On the Mac, you have vim available by default, and can get a GUI version in [MacVim](http://macvim-dev.github.io/macvim/). Many people like [TextWrangler](http://www.barebones.com/products/textwrangler/) and [Sublime Text](https://www.sublimetext.com/3) is becoming very popular.

On Linux you of course have vi and emacs, and gedit and nano are widely available.