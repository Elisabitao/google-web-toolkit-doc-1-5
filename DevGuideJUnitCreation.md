#summary An example of how to create JUnit test cases for your GWT project

# Creating a Test Case #

This section will describe how to create and run a set of unit test cases for your GWT project.  In order to use this facility, you must have the [JUnit](http://sourceforge.net/projects/junit/) library installed on your system.


## The GWTTestCase Class ##

GWT includes a special [GWTTestCase](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html) base class that provides [JUnit](http://www.junit.org) integration. Running a compiled [GWTTestCase](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html) subclass under JUnit launches an invisible instance of the GWT [hosted mode browser](DevGuideHostedMode.md) which serves to emulate your application behavior during test execution.

GWTTestCase is derived from JUnit's [TestCase](http://junit.sourceforge.net/javadoc/junit/framework/TestCase.html).  The typical way to setup a JUnit test case class is to have it extend `TestCase`, and then run the it with the JUnit [TestRunner](http://junit.sourceforge.net/javadoc/junit/awtui/TestRunner.html).  `TestCase` uses reflection to discover the test methods defined in your derived class.  It is convention to begin the name of all test methods with the prefix `test`.


## Using junitCreator ##

GWT includes a [junitCreator](DevGuideJunitCreator.md) tool that can generate a starter test case for you, plus scripts for testing in both hosted mode and web mode.

The following is an example of creating a test class using the [junitCreator](DevGuideJunitCreator.md) that implements _GWTTestCase_.  The example assumes that you have already created a module named `com.example.foo.Foo`:

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

After creating the skeleton of the test case and the launch file, add your testing logic to the generated `FooTest.java` class file.  Make sure to compile the class using `javac` or your favorite IDE.

Running `FooTest-hosted` tests the methods in `test/com/example/foo/client/FooTest.java` as Java bytecode in a JVM. `FooTest-web` tests as compiled JavaScript. The launch configurations do the same thing in Eclipse.

## Creating a Test Case by Hand ##

If you prefer not to use the [junitCreator](DevGuideJunitCreator.md), you may create a test case suite by hand by following the instructions below:


  1. **Define a class that extends [GWTTestCase](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html).**  Make sure your test class is on the module source path (e.g. in the `client` subpackage of your module.)  You can add new source paths by editing the [module XML file](DevGuideModuleXml.md) and adding a `<source>` element.
  1. **If you do not have a GWT module yet, create a [module](DevGuideModules.md) that causes the source for your test case to be included.** If you are adding a test case to an existing GWT app, you can just use the existing module.
  1. **Implement the method [GWTTestCase.getModuleName()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html#getModuleName()) to return the fully-qualified name of the module.**  This is the glue that tells the JUnit Test case which module to instantiate.
  1. **Compile your test case class to bytecode.**  You can use the Java compiler directly using [javac](http://java.sun.com/j2se/1.4.2/docs/tooldocs/windows/javac.html) or a Java IDE such as [Eclipse](http://eclipse.org).
  1. **Run your test case.**  Use the class `junit.textui.TestRunner` as your main class and pass the full name of your test class as the command line argument, e.g. `com.example.foo.client.FooTest`.   When running the test case, make sure your classpath includes:
    * Your project's `src` directory
    * Your project's `bin` directory
    * The `gwt-user.jar` library
    * One of `gwt-dev-windows.jar`, `gwt-dev-linux.jar`, or `gwt-dev-mac.jar` depending on your platform
    * The `junit.jar` library

## Client side Example ##

First of all, you will need a valid GWT module to host your test case class.  Usually, you do not need to create a new [module XML file](DevGuideModuleXml.md) - you can just use the one you have already created to develop your GWT module.  If you do not already have a `com.example.foo.Foo` module, create it as follows:

```
<module>
  <!-- Module com.example.foo.Foo -->

  <!-- Standard inherit.                                           -->
  <inherits name='com.google.gwt.user.User'/>

  <!-- implicitly includes com.example.foo.client package          -->

  <!-- OPTIONAL STUFF FOLLOWS -->

  <!-- It's okay for your module to declare an entry point.        -->
  <!-- This gets ignored when running under JUnit.                 -->
  <entry-point class='com.example.foo.FooModule'/>

  <!-- You can also test remote services during a JUnit run.       -->
  <servlet path='/foo' class='com.example.foo.server.FooServiceImpl'/>
</module>
```

> _Tip: You do not need to create a separate module for every test case. In the example above, any test cases in `com.example.foo.client` (or any subpackage) can share the `com.example.foo.Foo` module.
>_


You need to be able to access the code that is to be unit tested through some public method.  In this example, the module `Foo` has a validation function that tests a string and returns `true` or `false`:

```
  /**
   * A sample routine that runs in the module that we want to run a 
   * unit test on.  Accepts the strings "foo" and "bar" in any case,
   * and the string "Baz" as an exact match.  All other strings fail.
   * 
   * @param input A string to validate
   * @return true if the string passes validation.
   */
  public boolean exampleValidator (String input) {
    if (input.toLowerCase().equals("foo")){
      return true;
    } else if (input.toLowerCase().equals("bar")) {
      return true;
    } else if (input.equals("Baz")) {
      return true;
    }
    return false;
  }

```

Here is an example implementation `com.example.foo.client.FooTest` test case that invokes the validation function with different input strings to make sure the implementation matches the specified behavior in the comment:

```
import com.google.gwt.junit.client.GWTTestCase;
import com.example.foo.client.Foo;

public class FooTest extends GWTTestCase {

  /**
   * Specifies a module to use when running this test case. The returned
   * module must cause the source for this class to be included.
   * 
   * @see com.google.gwt.junit.client.GWTTestCase#getModuleName()
   */
  public String getModuleName() {
    return "com.example.foo.Foo";
  }
 
  /**
   * Test the method 'exampleValidator()' in module 'Foo'
   */
  public void testExampleValidator() {
    Foo myModule = new Foo();
    
    assertTrue(myModule.exampleValidator("foo"));
    assertTrue(myModule.exampleValidator("Foo"));
    assertTrue(myModule.exampleValidator("FOO"));
    assertTrue(myModule.exampleValidator("fOo"));
    assertTrue(myModule.exampleValidator("bar"));
    assertTrue(myModule.exampleValidator("Baz"));
    
    assertFalse(myModule.exampleValidator("fooo"));
    assertFalse(myModule.exampleValidator("oo"));
    assertFalse(myModule.exampleValidator("baz"));
  }

```

Now, there are several ways to invoke your test, depending on how you created the unit test and your environment.

If you created your unit tests with [junitCreator](DevGuideJunitCreator.md), execute your test with the command `FooTest-hosted`:

```
$PP_OFF
$ ./FooTest-hosted
..
Time: 12.679

OK (1 test)
```

If you did not use the  [junitCreator](DevGuideJunitCreator.md) tool, you can create your own script similar to what the tool would have created.  Here is the script that the tool created to run tests in hosted mode for Linux:

```
$PP_OFF
#!/bin/sh
APPDIR=`dirname $0`;
java  -Dgwt.args="-out www-test" -cp "$APPDIR/src:$APPDIR/test:$APPDIR/bin:
        /opt/eclipse/plugins/org.junit4_4.3.1/junit.jar:
        /home/user/gwt/gwt-linux-1.4.60/gwt-user.jar:
        /home/user/gwt/gwt-linux-1.4.60/gwt-dev-linux.jar"
        junit.textui.TestRunner com.example.client.FooTest "$@";
```

And here is an example script to launch the test on Windows:

```
$PP_OFF
@java -Dgwt.args="-out www-test" -cp "%~dp0\src;%~dp0\test;%~dp0\bin;
        C:\eclipse\plugins\org.junit_3.8.2.v200706111738\junit.jar;
        C:/gwt/gwt-windows-1.4.60/gwt-user.jar;
        C:/gwt/gwt-windows-1.4.60/gwt-dev-windows.jar" 
        junit.textui.TestRunner com.example.client.FooTest %*
```

Some IDEs have direct support for the JUnit testing framework.  When run with the `-eclipse` argument, the [junitCreator](DevGuideJunitCreator.md) tool optionally creates a `.launch` configuration file that you can invoke from the `Open Run Dialog...` menu option in [Eclipse](http://eclipse.org).  The configuration appears under the _JUnit_ heading.

## Running your test in Web Mode ##

When using the [junitCreator](DevGuideJunitCreator.md) tool, several command-line scripts are created to launch your tests in either hosted mode or web mode.  Make sure you test in both modes - although rare, there are [some differences between Java and JavaScript](DevGuideJavaCompatibility.md) that could cause your code to produce different results when deployed.

If you do not use the [junitCreator](DevGuideJunitCreator.md) tool and instead decide to run the JUnit TestRunner from command line, you must add some additional arguments get your unit tests running.  By default, tests run in [hosted mode](DevGuideHostedMode.md) are run as normal Java bytecode in a JVM. Overriding this default behavior requires passing arguments to the GWT shell. Arguments cannot be passed directly through the command-line, because normal command-line arguments go directly to the JUnit runner. Instead, define the system property `gwt.args` to pass arguments to GWT. For example, to run in [web mode](DevGuideWebMode.md), declare `-Dgwt.args="-web"` as a JVM argument when invoking JUnit. To get a full list of supported options, declare `-Dgwt.args="-help"` (instead of running the test, help is printed to the console).

## Running your test in Manual Mode ##

Manual mode tests allow you to run unit tests manually on any browser.  This mode was introduced in the GWT 1.5 release.  In this mode, the `JUnitShell` main class runs as usual on a specified GWT module, but instead of running the test immediately, it prints out a URL and waits for a browser to connect.  You can manually cut and paste this URL into the browser of your choice, and the unit tests will run in that browser.

Manual mode test scripts are not generated by the tool, but you can easily create one by copying the `<module-name>-web` script and replacing the `-web` with the `-manual` argument in the `-Dgwt.args` part of the script.

## Automating your Test Cases ##

When developing a large project, a good practice is to integrate the running of your test cases with your regular build process.  When you build manually, such as using `ant` from the command line or using your desktop IDE, this is as simple as just adding the invocation of JUnit into your regular build process.  When building in a server environment, there are a few extra considerations.

As mentioned before, when you run `GWTTestCase` tests, there is an invisible browser running. However, even though the browser is invisible, the test case classes still require the GUI libraries to run.  If you need to run unit test cases on a headless server (usually as a part of an automated build), make sure the same GUI libraries that would be used to run GWT hosted mode are installed on the server.  On Linux, you can run the X Virtual Framebuffer (Xvfb) to be a target for the invisible browser before launching your tests.

Also, consider organizing your tests into [GWTTestSuite](DevGuideJUnitSuites.md) classes to get the best performance from your unit tests.

## Server side testing ##

The tests described above are intended to assist with testing client side code.  The test case wrapper `GWTTestCase` will launch either a hosted mode session or a web browser to test the generated JavaScript.  On the other hand, server side code runs as native Java in a JVM without being translated to JavaScript, so it is not appropriate to run tests of server side code using 'GWTTestCase' as the base class for your tests. Instead, use JUnit's `TestCase` and other related classes directly when writing tests for your application's server side code.

### See Also ###

  * [JUnit Integration](DevGuideJUnitIntegration.md)

  * [Getting Started Tutorial - JUnit](GettingStartedJUnit.md)
