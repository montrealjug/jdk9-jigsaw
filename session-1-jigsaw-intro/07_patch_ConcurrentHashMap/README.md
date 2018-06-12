### --patch-module example

Developers that checkout `java.util.concurrent` classes from Doug Lea's CVS will be used to compiling the source files and deploying those classes with `-Xbootclasspath/p`.
`-Xbootclasspath/p` has been removed from Java, and its module replacement is the option `--patch-module` to override classes in a module. It can also be used to augment the contents of module. 
The `--patch-module` option is also supported by `javac` to compile code "as if" part of the module.

See the contents of the respective sources contained in the `src` folder:

- module `java.base` in `mypatches` folder
- module `com.greetings` in `mods` folder
- module `java.base` in `src` folder

    ```
    $ cat src/java.base/java/util/concurrent/ConcurrentHashMap.java
    ```

- module `com.greetings` in `src` folder

    ```
    $ cat src/com.greetings/com/greetings/Main.java
    ```

Both the modules are compiled from the sources into the folders `mypatch` and `mods` respectively with the following command:

    $ javac --patch-module java.base=src \
        -d mypatches/java.base \
        src/java.base/java/util/concurrent/ConcurrentHashMap.java

    $ javac --module-path mods \
        -d mods/com.greetings/ \
        src/com.greetings/module-info.java \
        src/com.greetings/com/greetings/Main.java


And we run the example twice (with and without the patched code) with the following command:
    
    $ java --module-path mods \
        --module com.greetings/com.greetings.Main

    $ java --module-path mods \
        --patch-module java.base=mypatches/java.base \
        --module com.greetings/com.greetings.Main

See [../01_Greetings/README.md](../01_Greetings/README.md) to learn more about package and module naming conventions and how to avoid confusions between them.
