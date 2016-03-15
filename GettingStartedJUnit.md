#summary Adding unit tests to the application.

# Test with JUnit #

Our StockWatcher application is pretty stable at the moment.  But what will happen when we start adding code for our 2.0 release (for example, the highly anticipated _future stock price prediction_ feature)?  How can we ensure that we don't break existing functionality as the code base evolves?  The solution is _unit tests_.

We can automate testing our GWT applications using the popular [JUnit](http://www.junit.org/) testing framework.  GWT includes a number of tools and classes which simplify the task of unit testing our applications in both [hosted mode](DevGuideHostedMode.md) and [web mode](DevGuideWebMode.md).  We'll use this functionality to add a simple unit test to StockWatcher.

## Write unit tests ##

First, we need to create classes containing our unit tests.  GWT has a command-line utility to get us started called [junitCreator](DevGuideJunitCreator.md).

### junitCreator ###

When invoking juintCreator, we'll need to supply a few arguments.  First, it needs the full path to the JUnit library.  Second, we must specify the name of the GWT [module](DevGuideModules.md) we're testing.  If we're using Eclipse, we can pass the name of our Eclipse project to have `junitCreator` create launch configurations for us.  Finally, we'll specify the fully-qualified name of of the test class to generate.

To generate a test class for StockWatcher, open a command shell,  browse to the main `StockWatcher` directory and then run `junitCreator` with the following arguments (_you'll probably need to modify the path to the JUnit library to point to the installation on your system_):

```
$PP_OFF
junitCreator -junit "C:\eclipse\plugins\org.junit_3.8.2.v200706111738\junit.jar" -module com.google.gwt.sample.stockwatcher.StockWatcher -eclipse StockWatcher com.google.gwt.sample.stockwatcher.client.StockWatcherTest
```

When `junitCreator` is finished, you should see a list of the directories and files it has created for you:

```
$PP_OFF
Created directory C:\workspace\StockWatcher\test
Created directory C:\workspace\StockWatcher\test\com\google\gwt\sample\stockwatcher\client
Created file C:\workspace\StockWatcher\test\com\google\gwt\sample\stockwatcher\client\StockWatcherTest.java
Created file C:\workspace\StockWatcher\StockWatcherTest-hosted.launch
Created file C:\workspace\StockWatcher\StockWatcherTest-web.launch
Created file C:\workspace\StockWatcher\StockWatcherTest-hosted.cmd
Created file C:\workspace\StockWatcher\StockWatcherTest-web.cmd
```

The first file we're interested in is the `StockWatcherTest.java` class it generated in the `com.google.gwt.sample.stockwatcher.client` package under the `test` directory.  This is the class that will contain our StockWatcher unit tests cases.  If you refresh your Eclipse project, you'll see all of the newly-added files:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherTestFiles.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherTestFiles.png)

Now take a look inside `StockWatcherTest.java`:

```
package com.google.gwt.sample.stockwatcher.client;

import com.google.gwt.junit.client.GWTTestCase;

/**
 * GWT JUnit tests must extend GWTTestCase.
 */
public class StockWatcherTest extends GWTTestCase {

  /**
   * Must refer to a valid module that sources this class.
   */
  public String getModuleName() {
    return "com.google.gwt.sample.stockwatcher.StockWatcher";
  }

  /**
   * Add as many tests as you like.
   */
  public void testSimple() {
    assertTrue(true);
  }

}
```

There a couple of things to notice about this automatically-generated test class.  Like all GWT JUnit test cases, it extends the [GWTTestCase](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html) class in the `com.google.gwt.junit.client` package.  This class has an abstract method named [getModuleName()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html#getModuleName()) that we must override.  You can probably guess that this method needs to return the name of the GWT module, which in our case is `com.google.gwt.sample.stockwatcher.StockWatcher`.

After that, we simply add our tests in the form of public methods.  The `junitCreator` tool has already provided us with one (tautological) test named `testSimple`.  This method uses one of the many `assert*` functions available to us from the JUnit [Assert](http://junit.sourceforge.net/javadoc/junit/framework/Assert.html) class, which is an ancestor of GWTTestCase.  The one we're using here, [assertTrue(boolean)](http://junit.sourceforge.net/javadoc/junit/framework/Assert.html#assertTrue(boolean)) simply asserts that the `boolean` argument passed in evaluates to `true`.  If not, the `testSimple` test will fail when run in JUnit.

### Adding test cases ###

Let's go ahead and add a real test for StockWatcher.  We'll create a new test named `testStockPriceCtor` to verify that the constructor of our StockPrice class is correctly setting the new object's instance fields.  Add this method to the StockWatcherTest class:

```
public void testStockPriceCtor() {
  String symbol = "XYZ";
  double price = 70.0;
  double change = 2.0;
  double changePercent = 100.0 * change / price;
  
  StockPrice sp = new StockPrice(symbol, price, change);
  assertNotNull(sp);
  assertEquals(symbol, sp.getSymbol());
  assertEquals(price, sp.getPrice(), 0.001);
  assertEquals(change, sp.getChange(), 0.001);
  assertEquals(changePercent, sp.getChangePercent(), 0.001);
}
```

You can see that we're using a few more of the `assert*` functions to check the dummy StockPrice object.  There are many different overloads of the [assertEquals](http://junit.sourceforge.net/javadoc/junit/framework/Assert.html#assertEquals(boolean,%20boolean)) method, which we can use to compare a value to an expected value.  One thing to notice here is the version which compares two `double`'s for equality.  In addition to the expected and actual values, it also takes a `delta` value that specifies how close the other two values have to be to be considered "equal" (recall that because of the way floating-point numbers are stored, you should generally never try to test them for exact equality).  Our equality tests in testStockPriceCtor() will pass as long as the actual value is within one-tenth of a cent of the expected value.

In a real testing scenario, we would add lots of unit tests to the StockWatcherTest class to verify the behavior of as much of StockWatcher as possible.  We would probably want to group those test cases in several different test classes, each generated by the `junitCreator` tool.  However, for our purposes of demonstration we'll stop at one test case, and move on now to running the tests.

## Running unit tests ##

There are a couple of ways you can run your completed JUnit tests.  One option is to use the scripts that were automatically generated by `junitCreator`: one for testing the application in hosted mode (`StockWatcherTest-hosted`) and another for web mode (`StockWatcherTest-web`).  If you look inside either of these scripts, you'll see that it launches the JUnit `junit.textui.TestRunner` class to actually run the tests.

Speaking of running tests, let's go ahead and run our StockWatcherTest test cases now.  In a command shell, browse to the `StockWatcher` directory and run the `StockWatcherTest-hosted` script.  After it completes, you should see output that looks something like this:

```
$PP_OFF
..
Time: 6.515

OK (2 tests)
```

Not a lot to see here, but the `OK` indicates that all went well with our unit tests (you'll see in a bit what happens when things _don't_ go as planned).

If your version of Eclipse as the JUnit plug-in installed, another option for running our test cases is to use the Eclipse launch configurations that `junitCreator` generated.  To use them, right-click the StockWatcher project in Eclipse and select _Run As -> Open Run Dialog..._ from the context menu.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherTestRunConfig.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherTestRunConfig.png)

If you expand the "JUnit" node in the tree you should find the two JUnit launch configurations.  You can select one and then click the _Run_ button to run the unit tests.  A special JUnit view will appear that indicates the status of the tests:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherJUnitEclipse.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherJUnitEclipse.png)

### Test case failures ###

Currently, StockWatcher is passing its unit tests with flying colors.  Let's see what happens when we "break" the application and cause one of our test cases to fail.

Remember the arithmetic bug we discovered back in the section on [hosted mode debugging](GettingStartedHostedMode.md)?  Let's reintroduce it back into the code by changing the getChangePercent() method in `StockPrice.java`:

```
public double getChangePercent() {
  return 10.0 * _change / _price;
}
```

Now, trying running the `StockWatcherTest-hosted` script again.  This time we get a lot more output than before:

```
$PP_OFF
..F
Time: 10.469
There was 1 failure:
1) testStockPriceCtor(com.google.gwt.sample.stockwatcher.client.StockWatcherTest)junit.framework.AssertionFailedError:  expected=0.02857142857142857 actual=0.2857142857142857 delta=0.0010
        at com.google.gwt.sample.stockwatcher.client.StockWatcherTest.testStockPriceCtor(StockWatcherTest.java:35)
        at com.google.gwt.sample.stockwatcher.client.__StockWatcherTest_unitTestImpl.doRunTest(transient source for com.google.gwt.sample.stockwatcher.client.__StockWatcherTest_unitTestImpl:11)
        at com.google.gwt.junit.client.impl.GWTTestCaseImpl.runTest(GWTTestCaseImpl.java:397)
        at com.google.gwt.junit.client.impl.GWTTestCaseImpl.access$1(GWTTestCaseImpl.java:387)
        at com.google.gwt.junit.client.impl.GWTTestCaseImpl$JUnitHostListener.onSuccess(GWTTestCaseImpl.java:67)
        at com.google.gwt.junit.client.impl.JUnitHost_Proxy$2.onCompletionImpl(transient source for com.google.gwt.junit.client.impl.JUnitHost_Proxy:135)
        at com.google.gwt.junit.client.impl.JUnitHost_Proxy$2.onCompletionAndCatch(transient source for com.google.gwt.junit.client.impl.JUnitHost_Proxy:111)
        at com.google.gwt.junit.client.impl.JUnitHost_Proxy$2.onCompletion(transient source for com.google.gwt.junit.client.impl.JUnitHost_Proxy:105)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(Unknown Source)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source)
        at com.google.gwt.dev.shell.ie.IDispatchImpl.callMethod(IDispatchImpl.java:126)
        at com.google.gwt.dev.shell.ie.IDispatchProxy.invoke(IDispatchProxy.java:150)
        at com.google.gwt.dev.shell.ie.IDispatchImpl.Invoke(IDispatchImpl.java:293)
        at com.google.gwt.dev.shell.ie.IDispatchImpl.method6(IDispatchImpl.java:196)
        at org.eclipse.swt.internal.ole.win32.COMObject.callback6(COMObject.java:117)
        at org.eclipse.swt.internal.win32.OS.DispatchMessageW(Native Method)
        at org.eclipse.swt.internal.win32.OS.DispatchMessage(OS.java:1925)
        at org.eclipse.swt.widgets.Display.readAndDispatch(Display.java:2966)
        at com.google.gwt.dev.GWTShell.pumpEventLoop(GWTShell.java:689)
        at com.google.gwt.junit.JUnitShell.runTestImpl(JUnitShell.java:442)
        at com.google.gwt.junit.JUnitShell.runTest(JUnitShell.java:167)
        at com.google.gwt.junit.client.GWTTestCase.runTest(GWTTestCase.java:194)
        at com.google.gwt.junit.client.GWTTestCase.run(GWTTestCase.java:114)

FAILURES!!!
Tests run: 2,  Failures: 1,  Errors: 0
```

You can see that JUnit identified the failed test case, `testStockPriceCtor` and provided us with a full-stack trace for the exception that resulted (an [AssertionFailedError](http://junit.sourceforge.net/javadoc/junit/framework/AssertionFailedError.html)).  If you now go back and return the getChangePecent() method to its original, working condition and rerun the JUnit tests they should all complete successfully again.

### Hosted mode vs. web mode ###

Because there may be subtle [differences](DevGuideJavaCompatibility.md) between the way GWT applications work when compiled to JavaScript and when running as Java bytecode in [hosted mode](DevGuideHostedMode.md), make sure you run your unit tests in _both_ modes as you develop your application.  If your test cases fail when running in web mode, though, you won't get the full stack trace that you see in hosted mode.

## More about testing ##

There's more to unit testing with GWT than we touched on here, including [asynchronous testing](DevGuideJUnitAsync.md) and performance [benchmarking](DevGuideJUnitBenchmarking.md).  For more details, see the Development Guide section on [JUnit integration](DevGuideJUnitIntegration.md).
