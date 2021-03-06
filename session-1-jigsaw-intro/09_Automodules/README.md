### External libraries example

In this example we include external libraries, which haven't been modularized so far.
These jar files can be translated into so called *[automatic modules](The State of the Module System)* by `javac`.
To get an idea of how those modules will look and which dependencies they will require you can use the `jar` tool:

    $ jar -d --file=lib/junit-4.12.jar
    No module descriptor found. Derived automatic module.
    
    module junit@4.12 (automatic)
      requires mandated java.base
       contains junit.extensions
       contains junit.framework
       contains junit.runner
       contains junit.textui
       contains org.junit
       ...

This output tells us, that the automatic module will be called `junit` and will have the version number 4.12.
It sadly doesn't tell us, that JUnit depends on Hamcrest.
We can get a hint towards that using `jdeps` however:

    $ jdeps -s lib/junit-4.12.jar 
    junit-4.12.jar -> java.base
    junit-4.12.jar -> java.management
    junit-4.12.jar -> not found

That last `not found` entry tells us that there is some unknown dependency.
The `-s` option requests that only the summary is printed.
So to take a closer look, let's omit that:

    $ jdeps lib/junit-4.12.jar | grep "not found"
    junit-4.12.jar -> not found
       org.junit                                          -> org.hamcrest                                       not found
       org.junit.experimental.results                     -> org.hamcrest                                       not found
       org.junit.internal                                 -> org.hamcrest                                       not found
       org.junit.internal.matchers                        -> org.hamcrest                                       not found
       org.junit.matchers                                 -> org.hamcrest                                       not found
       org.junit.matchers                                 -> org.hamcrest.core                                  not found
       org.junit.rules                                    -> org.hamcrest                                       not found


Aha! Hamcrest it is. And we also see, that there are no other unknown dependencies from JUnit. Let's just check Hamcrest to be sure:

    $ jar -d --file=lib/hamcrest-core-1.3.jar
    o module descriptor found. Derived automatic module.
    
    module hamcrest.core@1.3 (automatic)
      requires mandated java.base
      contains org.hamcrest
      contains org.hamcrest.core
      contains org.hamcrest.internal

And

    $ jdeps -s lib/hamcrest-core-1.3.jar
    hamcrest-core-1.3.jar -> java.base

So, Hamcrest has no further dependencies.

Perform the below commands to see the contents of the respective sources contained in the `src` folder:

    $ cat src/com.greetings/module-info.java
    $ cat src/com.greetings/main/java/com/greetings/Main.java
    $ cat src/com.greetings/main/java/com/greetings/Greet.java
    $ cat src/com.greetings/test/java/com/greetings/GreetTest.java 

As you can see in the `module-info.java`, we have added a dependency to JUnit using the module name retrieved with the above commands.
Hamcrest is a transitive dependency and as such does not have to be added separately.
Also, since JUnit will have to access our classes to run tests, we export `com.greetings` to the JUnit module.

You can see the dependency information by running the command:

    $ jar -d --file=lib/junit-4.12.jar
    $ jdeps -s lib/junit-4.12.jar

Try to compile the modules using the below command:

    $ javac --module-path lib \
        -d mods/main/com.greetings \
        src/com.greetings/module-info.java \
        src/com.greetings/main/java/com/greetings/Greet.java \
        src/com.greetings/main/java/com/greetings/Main.java
    
    # For Windows
    $ javac --module-path "mods;lib" \
          -d mods/test/com.greetings \
          src/com.greetings/module-info.java \
          src/com.greetings/main/java/com/greetings/Greet.java \
          src/com.greetings/test/java/com/greetings/GreetTest.java

    # Linux or MacOs
    javac --module-path mods:lib \
          -d mods/test/com.greetings \
          src/com.greetings/module-info.java \
          src/com.greetings/main/java/com/greetings/Greet.java \
          src/com.greetings/test/java/com/greetings/GreetTest.java

And we run the example with the following command:
    
    *** Running from within the mods folder without arguments. *** 
    
    # For Windows
    $ java --module-path "mods/main;lib" \
         --module com.greetings/com.greetings.Main

    # For Linux or MacOs
    java --module-path mods/main:lib \
         --module com.greetings/com.greetings.Main
    
    
    *** Running from within the mods folder with arguments. *** 
    
    # For Windows
    $ java --module-path "mods/main;lib" \
         --module com.greetings/com.greetings.Main \
        Alice Bob Charlie

    # For Linux or MacOs
    $ java --module-path mods/main:lib \
         --module com.greetings/com.greetings.Main \
        Alice Bob Charlie


    *** Running tests *** 
    
    # For Windows
    $ java --module-path "mods/main;lib" \
         --add-modules com.greetings \
         --patch-module com.greetings=mods/test/com.greetings \
         --module junit/org.junit.runner.JUnitCore \
        com.greetings.GreetTest

    # For Linux or MacOs
    java --module-path mods/main:lib \
         --add-modules com.greetings \
         --patch-module com.greetings=mods/test/com.greetings \
         --module junit/org.junit.runner.JUnitCore \
        com.greetings.GreetTest


```
    
Check the contents of this script file (use the `cat` command or a text editor) to see what they are doing and why - interesting instructions and information in there.

See [../01_Greetings/README.md](../01_Greetings/README.md) to learn more about package and module naming conventions and how to avoid confusions between them.