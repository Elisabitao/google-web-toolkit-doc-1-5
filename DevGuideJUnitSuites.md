# Combining TestCase classes into a TestSuite #

The [GWTTestSuite](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/tools/GWTTestSuite.html) mechanism has the overhead of having to start a hosted mode shell and servlet or compile your code.  You can potentially increase the speed of your unit test runs by organizing your tests into suites of test cases.

Combining test cases together into a single  [GWTTestSuite](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/tools/GWTTestSuite.html)  reduces overhead if multiple [GWTTestCase](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html) classes share the same module (that is, they return the same value from [getModuleName()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html#getModuleName()).)  The [GWTTestSuite](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/tools/GWTTestSuite.html) class re-orders the test cases so that all cases that share a module are run back to back.

Creating a suite is simple if you have already defined individual JUnit  [TestCases](http://junit.sourceforge.net/javadoc/junit/framework/TestCase.html) or  [GWTTestCases](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html).  Here is an example:

```

public class MapsTestSuite extends GWTTestSuite {
  public static Test suite() {
    TestSuite suite = new TestSuite("Test for a Maps Application");
    suite.addTestSuite(MapTest.class); 
    suite.addTestSuite(EventTest.class);
    suite.addTestSuite(CopyTest.class);
    return suite;
  }
}
```

The three test cases `MapTest`, `EventTest`, and `CopyTest` can now all run in the same instance of JUnitShell.

```
java  -Xmx256M -cp "./src:./test:./bin:./junit.jar:/gwt/gwt-user.jar:/gwt/gwt-dev-linux.jar:/gwt/gwt-maps.jar" junit.textui.TestRunner com.example.MapsTestSuite
```
