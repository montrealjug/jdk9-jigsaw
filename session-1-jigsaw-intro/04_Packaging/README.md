### Packaging example

In the examples so far then the contents of the compiled modules were exploded on the file system. For transportation and deployment purposes then it is usually more convenient to package a module as a modular JAR. 

A modular JAR is a regular JAR file that has a `module-info.class` in its top-level directory, it creates `org.astro@1.0.jar` and `com.greetings.jar` in directory `mlib`.

The modules are compiled from the sources with the following commands:

    $ mvn compile

Now create the packages (jar files) with the below command:

    $ jar --create \
        --file mlib/com.greetings.jar \
	    --main-class=com.greetings.Main \
	    -C com.greetings/target/classes .

    $ jar --create \
        --file mlib/org.astro@1.0.jar \
	    --module-version 1.0 \
	    -C org.astro/target/classes .

    
You will notice that the module `org.astro` is packaged to indicate that its version is `1.0`. Module `com.greetings` has been packaged to indicate that its main class is `com.greetings.Main`. Module `com.greetings` can be executed without needing to specify its main class (`Main.class`).
    
And we run the example with the following command:
    
    $ java --module-path mlib --module com.greetings
    
See [../01_Greetings/README.md](../01_Greetings/README.md) to learn more about package and module naming conventions and how to avoid confusions between them.
