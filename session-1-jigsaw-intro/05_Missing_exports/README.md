### Missing exports example

In this example we mistakenly omit the `exports` from the `com.astro` module declaration (in `module-info.java`).

    $ cat org.astro/src/main/java/module-info.java
    
The sources are compiled into the folders `org.astro/target/classes/org.astro` and `com.greetings/target/classes/com.greetings` with the following commands:

    $ mvn compile
    
Compilation fails with the below error:

```
Compilation failure
[ERROR] /Users/xbouclet/Sources/jdk9-jigsaw/session-1-jigsaw-intro/05_Missing_exports/com.greetings/src/main/java/com/greetings/Main.java:[3,11] package org.astro is not visible
[ERROR]   (package org.astro is declared in module org.astro, which does not export it)
```
    
Check the contents of this script file (use the `cat` command or a text editor) to see what they are doing and why - interesting instructions and information in there.

See [../01_Greetings/README.md](../01_Greetings/README.md) to learn more about package and module naming conventions and how to avoid confusions between them.
