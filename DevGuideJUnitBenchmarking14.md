# Benchmarking for GWT 1.4 #

> _Note: This page describes JUnit benchmarking in the GWT 1.4 release.  For documentation on using the current version of benchmarking, see [DevGuideJUnitBenchmarking](DevGuideJUnitBenchmarking.md)._

GWT's [JUnit](http://www.junit.org) integration provides special support for creating and reporting on benchmarks. Specifically, GWT has contains a [Benchmark](http://google-web-toolkit.googlecode.com/svn/javadoc/1.4/com/google/gwt/junit/client/Benchmark.html) class which provides built-in facilities for common benchmarking needs.  To take advantage of benchmarking support, take the following steps:
  1. Review the documentation on [Benchmark](http://google-web-toolkit.googlecode.com/svn/javadoc/1.4/com/google/gwt/junit/client/Benchmark.html). Take a look at the example benchmark code.
  1. Create your own benchmark by subclassing [Benchmark](http://google-web-toolkit.googlecode.com/svn/javadoc/1.4/com/google/gwt/junit/client/Benchmark.html). Execute your benchmark like you would any normal JUnit test. By default, the test results are written to a report XML file in your working directory.
  1. Run [benchmarkViewer](DevGuideBenchmarkViewer.md) to browse visualizations (graphs/charts) of your report data. The `benchmarkViewer` is a GWT tool in the root of your GWT installation directory that displays benchmark reports.

## Example ##

To use the benchmarking classes, start by using `junitCreator` to create a skeleton JUnit class and launch configuration.

```
 > junitCreator -junit ~/tools/lib/junit/junit-3.8.1.jar -module com.example.foo.Foo -eclipse FooBenchmark com.example.foo.test.FooBenchmark
Created directory /home/user/workspace/Foo/test/com/example/client/test
Created file /home/user/workspace/Foo/test/com/example/foo/client/FooBenchmark.java
Created file /home/user/workspace/Foo/FooBenchmark-hosted.launch
Created file /home/user/workspace/Foo/FooBenchmark-web.launch
Created file /home/user/workspace/Foo/FooBenchmark-hosted
Created file /home/user/workspace/Foo/FooBenchmark-web
```

Now, edit `FooBenchmark.java`:

  * Add an import for `com.google.gwt.junit.client.Benchmark`
  * Change the base class from `GWTTestCase` to `Benchmark`

A Benchmark class differs from a JUnit class in that the test methods will be called repeatedly taking different values each time.  The Benchmark class is very flexible in this regard - you can specify as many parameters as you like, whereas you must leave a method definition with no arguments to satisfy the JUnit specification.

For each parameter, you will need to provide a single constant containing values to iterate over during the test.  GWT provides some built in classes to help define such ranges such as [IntRange](http://google-web-toolkit.googlecode.com/svn/javadoc/1.4/com/google/gwt/junit/client/IntRange.html). For example, this represents the values 512, 1024, 2048, 4096, ... :

```
     final IntRange baseRange = new IntRange(512, Integer.MAX_VALUE,
      Operator.MULTIPLY, 2);
```

You can create any range of values you like with whatever object values returned, as long as you specify an instance class of `Iterable` in the annotation for the function parameter.  The syntax for the `@gwt.benchmark.param` annotation is `<param name> = <Iterable>`. Optionally, you can add `-limit` to indicate to GWT that it should stop executing additional permutations of the test when the execution time becomes too long (over 1000ms):

```
  /**
   * @gwt.benchmark.param size -limit = baseRange
   */
```

In this case, `size` is a parameter to the test function, and the range of values are defined by the `Iterable` instance named `baseRange`.  The `-limit` flag tells the test to stop executing if any one run takes a long time, even if it hasn't run out of values to plug in from `baseRange`.

Here is a full example of a simple `Benchmark` subclass that compares the execution time of adding an element to a `Vector` to adding an element to an `ArrayList`:

```
package com.example.foo.client;

import com.google.gwt.junit.client.Benchmark;
import com.google.gwt.junit.client.GWTTestCase;
import com.google.gwt.junit.client.IntRange;
import com.google.gwt.junit.client.Operator;

import java.util.ArrayList;
import java.util.List;
import java.util.Vector;

/**
 * GWT JUnit tests must extend GWTTestCase.
 */
public class FooBenchmark extends Benchmark {

  final IntRange baseRange = new IntRange(512, Integer.MAX_VALUE,
      Operator.MULTIPLY, 2);
  private List testVector = new Vector();
  private List testList = new ArrayList();
  
  /**
   * Must refer to a valid module that sources this class.
   */
  public String getModuleName() {
    return "com.example.foo.Foo";
  }

  /**
   * JUnit requires a method that takes no arguments.  It is not used for 
   * Benchmarking.
   */
  public void testSimpleVector() {
  }
  
  /**
   * Test adding an element to a vector 
   * @gwt.benchmark.param size -limit = baseRange
   */
  public void testSimpleVector(Integer size)
  {
    int len = size.intValue();
    testVector.clear();
    for (int i=0 ; i<len; i++) {
      testVector.add(size);
    }
  }
  
  public void testSimpleList() {
  }
  
  /**
   * Test adding an element to a List
   * @gwt.benchmark.param size -limit = baseRange
   */
  public void testSimpleList(Integer size)
  {
    int len = size.intValue();
    testList.clear();
    for (int i=0 ; i<len; i++) {
      testList.add(size);
    }
  }
}
```


Now, run the script `FooBenchmark-hosted` that was created by `junitCreator` to run the benchmark.  The script will run your code in hosted mode and create a file named `report-*.xml`.  You can view this report with the `benchmarkViewer` command from command line:

> ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/BenchmarkReport3.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/BenchmarkReport3.png)


> ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/BenchmarkReport2.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/BenchmarkReport2.png)


For more advanced usage, see the [Benchmark](http://google-web-toolkit.googlecode.com/svn/javadoc/1.4/com/google/gwt/junit/client/Benchmark.html) class documentation.
