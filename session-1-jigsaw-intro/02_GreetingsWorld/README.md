### Greetings world example

This second example updates the module declaration to declare a dependency on module `org.astro`. Module `org.astro` exports the API package `org.astro`. 

Perform the below commands to see the contents of the respective Java classes contained in the `src` folder:

    $ cat src/main/java/org.astro/module-info.java

    $ cat src/main/java/org.astro/org/astro/World.java

Module `org.astro` exports the package `org.astro` (see `src/main/java/org.astro/module-info.java` for more details).

    $ cat src/main/java/com.greetings/module-info.java

    $ cat src/main/java/com.greetings/com/greetings/Main.java

Module `com.greetings` imports the package `org.astro` (see `src/main/java/com.greetings/module-info.java` for more details).

The sources are compiled into the folders `org.astro/target/classes/org.astro` and `com.greetings/target/classes/com.greetings` with the following commands:

    $ mvn compile
    
And we run the example with the following command:
    
    $ mvn -f ./com.greetings/pom.xml exec:exec
    
This time it doesn't work because the package `org.astro` is not in available in the maven's dependencies.

To add them :

    $ mvn install
    
And to run use the same command as before :

    $ mvn -f ./com.greetings/pom.xml exec:exec    
    
Check the contents of both these script files (use the `cat` command or a text editor) to see what they are doing and why - interesting instructions and information in there.

See [../01_Greetings/README.md](../01_Greetings/README.md) to learn more about package and module naming conventions and how to avoid confusions between them.
