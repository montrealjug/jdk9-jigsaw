### Multi-module compilation example

In this example modules are compiled at the same time unlike with the previous example where the compilation happens sequentially.

The sources are compiled into the folders `mods/org.astro` and `mods/com.greetings` with the following commands:

    $ mvn compile

Inspect both the modules created in the mods folder:

    
And we run the example with the following command:
    
    $ mvn install
    $ mvn -f ./com.greetings/pom.xml exec:exec

    or 

    $ java --module-path mods --module com.greetings/com.greetings.Main
    
Check the contents of all these script files (use the `cat` command or a text editor) to see what they are doing and why - interesting instructions and information in there.

See [../01_Greetings/README.md](../01_Greetings/README.md) to learn more about package and module naming conventions and how to avoid confusions between them.
