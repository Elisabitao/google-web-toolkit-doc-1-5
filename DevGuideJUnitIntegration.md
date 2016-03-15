# Unit Testing #

Creating a battery of good unit test cases is an important part of ensuring the quality of your application over its lifecycle.  To aid developers with their testing efforts, GWT provides integration with the popular [JUnit](http://www.junit.org) unit testing framework.  GWT allows JUnit test cases to run in either hosted mode or web mode, and also provides a benchmarking facility to help you understand the performance characteristics of the components of your application.

## Specifics ##
[Creating a JUnit Test Case](DevGuideJUnitCreation.md)
An example of how to create a JUnit test Case

[Asynchronous Testing](DevGuideJUnitAsync.md)
How to test event-driven features such as server calls or timers.

[Using Test Suites](DevGuideJUnitSuites.md)
Using Test Suites can speed up running unit tests.

[Prepare for and tear down after test cases](DevGuideJUnitSetUp.md)
How to prepare for and tearing down after JUnit test cases.

[Benchmarking](DevGuideJUnitBenchmarking.md)
How to use GWT's JUnit support to create and report on benchmarks to help you identify performance hot spots and optimize your code.
