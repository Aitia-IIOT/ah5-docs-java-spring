# FAQ

## Java Memory Usage

The Java heap is the area of memory used to store objects instantiated by the applications running on the Java Virtual Machine (JVM). The Maximum Java Heap Size (**Xmx**) is the maximum amount of memory that a Java application can use. Objects in the heap can be shared between threads. Each thread in a Java application has its own stack (**Xss**). The stack is also used to hold return addresses, function/method call arguments, etc. JVM also keeps track of metadata of the classes (**MaxMetaspaceSize**) which have been loaded separated from the main Java heap.

### Deafult Memory Usage

The default values for `Xmx` is based on the physical memory of the machine. From Java 11 the `Xmx` value is 25% of the available memory with a maximum of 25 GB. However, where there is 2 GB or less of physical memory, the value set is 50% of available memory with a minimum value of 16 MB and a maximum value of 512 MB.

The default value for `Xss` is 320 KB for 31-bit or 32-bit JVMs and 1024 KB for 64-bit JVMs.

The `MaxMetaspaceSize` is unlimited by default.

### Customized Memory Usage

Use these java command-line parameters to control the memory usage:

* Use `-Xmx` to specify the maximum Java heap size
* Use `-Xms` to specify the initial/minimum Java heap size
* Use `-Xss` to set the Java thread stack size
* Use `-XX:MaxMetaspaceSize` to set the maximum size of the Metaspace.

**Example** to specify the amount of memory the JVM should use when starting a core system:

`java -jar arrowhead-{core-system}-{version}.jar -Xms128M -Xmx1G`

_JVM will startup with 128 megabytes of memory and will allow the process to use up to 1 gigabyte of memory_

> _**Note:**_ The higher traffic is expected, the more memory will be necessary. A lower Xmx value will cause a decrease in performance due to JVM has to force frequent garbage collections in order to free up space, also if the Xmx value is lower than the amount of live data, it might trigger OutOfMemoryError.

[Learn more](https://docs.oracle.com/cd/E15289_01/JRPTG/)

## MySQL Connections

If your Core Systems gets a [Too many connections](https://dev.mysql.com/doc/refman/8.0/en/too-many-connections.html) error when try to connect to the mysqld server at start up, this means that all available connections are in use by other clients.

The number of connections permitted is controlled by the `max_connections` system variable. Its default value may vary from version to version. If you need to support more connections, you should set a larger value for this variable. See [MySQL Reference](https://dev.mysql.com/doc/refman/8.0/en/connection-interfaces.html#connection-interfaces-volume-management).