# junitCreator #


A command-line tool that generates a [JUnit test](DevGuideJUnitIntegration.md) and scripts for testing in both [hosted mode](DevGuideHostedMode.md) and [web mode](DevGuideWebMode.md).  Use these scripts as a starting point for building your own JUnit test suites based on [GWTTestCase](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html) or [Benchmark](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/Benchmark.html) subclass.

```
$PP_OFF
junitCreator -junit pathToJUnitJar [-eclipse projectName] [-out dir] [-overwrite] [-ignore] [-addToClassPath] className
```


|   `-junit`  |  Specify the path to your junit.jar (required) |
|:------------|:-----------------------------------------------|
|   `-module`  |  Specify name of the application module to use (required) |
|   `-eclipse`  |  Creates a debug launch configuration for the named eclipse project |
|   `-out`    |  The directory to write output files into (defaults to current) |
|   `-overwrite`  |  Overwrite any existing files                  |
|   `-ignore`  |  Ignore any existing files; do not overwrite   |
|   `-addToClassPath` |  Adds extra elements to the class path of files in the skeleton. |
|   `className`  |  The fully-qualified name of the test class to create |

## Example ##

The following is an example of creating a test class that implements GWTTestCase.  The example assumes that you have already created a module named `com.example.foo.Foo`:

```
$PP_OFF
 ~/Foo> junitCreator -junit /opt/eclipse/plugins/org.junit_3.8.1/junit.jar
        -module com.example.foo.Foo
        -eclipse Foo com.example.foo.client.FooTest
 Created directory test/com/example/foo/test
 Created file test/com/example/foo/client/FooTest.java
 Created file FooTest-hosted.launch
 Created file FooTest-web.launch
 Created file FooTest-hosted
 Created file FooTest-web
```

After creating the skeleton and launch file, add your testing logic to the generated class file `FooTest.java`.  Make sure to compile the class using `javac` or your IDE.

Running `FooTest-hosted` tests the methods in `test/com/example/foo/client/FooTest.java` as Java bytecode in a JVM. `FooTest-web` tests as compiled JavaScript. The launch configurations do the same thing in Eclipse.

### See Also ###

  * [JUnit Integration](DevGuideJUnitIntegration.md)

  * [Creating a JUnit Test Case](DevGuideJUnitCreation.md)

  * [Benchmarking](DevGuideJUnitBenchmarking.md)
