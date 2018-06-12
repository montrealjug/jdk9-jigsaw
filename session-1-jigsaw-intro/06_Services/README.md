### Services example

Services allow for loose coupling between service consumers modules and service providers modules.

This example has a service consumer module and a service provider module:

- module `com.socket` exports an API for network sockets. 
  The API is in package `com.socket` so this package is exported. 
  The API is plug-able to allow for alternative implementations. 
  The service type is class `com.socket.spi.NetworkSocketProvider` in the same module and thus package `com.socket.spi` is also exported.

- module `org.fastsocket` is a service provider module. 
  It provides an implementation of `com.socket.spi.NetworkSocketProvider`. 
  It does not export any packages. 
  
Inspect the respective sources contained in the `src` folder:

- module `com.socket`
    
    ```
    src/com.socket/module-info.java
    src/com.socket/com/socket/NetworkSocket.java
    src/com.socket/com/socket/spi/NetworkSocketProvider.java
    ```
    
- module `org.fastsocket`

    ```
    src/org.fastsocket/module-info.java
    src/org.fastsocket/org/fastsocket/FastNetworkSocketProvider.java
    src/org.fastsocket/org/fastsocket/FastNetworkSocket.java
    ```
    
- module `com.greetings`
    ```
    src/com.greetings/module-info.java
    src/com.greetings/com/greetings/Main.java
    ```

Both the modules are compiled together from the sources into the folder `mods` with the following commands:
    
    $ javac -d mods \
            --module-source-path src \
            $(find src -name "*.java")
            
    $ javac -d mods/com.greetings/ \
            --module-path mods \
            $(find src/com.greetings/ -name "*.java")
    
And we run the example with the following command:
    
    $ java --module-path mods \
           --module com.greetings/com.greetings.Main

See [../01_Greetings/README.md](../01_Greetings/README.md) to learn more about package and module naming conventions and how to avoid confusions between them.
