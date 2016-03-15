# OutOfMemoryError: Java heap space #
When compiling a project in GWT 1.5 or later, you may encounter problems with running out of memory.

```
$PP_OFF
Compiling permutations
   Analyzing permutation #1
      [ERROR] An internal compiler exception occurred
com.google.gwt.dev.jjs.InternalCompilerException: Unexpected error during visit.
        at ...
Caused by: java.lang.OutOfMemoryError: Java heap space
```

GWT 1.5 provides a larger library (in gwt-user.jar) that must be analyzed during compilation and a number of compiler optimizations that require more memory and processing power. Thus, the GWT compiler and hosted mode use more memory internally. This can cause the compiler to exceed the default JVM heap size.

Some projects need to increase JVM max memory to successfully compile; others may see improved compile times by increasing the limit.

### Workaround ###
Increase the Java heap limit using the Java VM argument -Xmx and specifying the amount of memory in megabytes. For example, `java -Xmx512M -cp [args...]` would set the heap max to 512 megabytes.
