# Asynchronous Testing #

GWT's [JUnit](http://www.junit.org) integration provides special support for testing functionality that cannot be executed in straight-line code. For example, you might want to make an [RPC](DevGuideRemoteProcedureCalls.md) call to a server and then validate the response. However, in a normal JUnit test run, the test stops as soon as the test method returns control to the caller, and GWT does not support multiple threads or blocking. To support this use case, [GWTTestCase](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html) has extended the `TestCase` API.
The two key methods are [GWTTestCase.delayTestFinish(int)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html#delayTestFinish(int)) and [GWTTestCase.finishTest()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html#finishTest()). Calling `delayTestFinish()` during a test method's execution puts that test in asynchronous mode, which means the test will not finish when the test method returns control to the caller. Instead, a _delay period_ begins, which lasts the amount of time specified in the call to `delayTestFinish()`. During the delay period, the test system will wait for one of three things to happen:
  1. If `finishTest()` is called before the delay period expires, the test will succeed.
  1. If any exception escapes from an event handler during the delay period, the test will error with the thrown exception.
  1. If the delay period expires and neither of the above has happened, the test will error with a [TimeoutException](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/TimeoutException.html).



The normal use pattern is to setup an event in the test method and call `delayTestFinish()` with a timeout significantly longer than the event is expected to take. The event handler validates the event and then calls `finishTest()`.



### Example ###

```
public void testTimer() {
  // Setup an asynchronous event handler.
  Timer timer = new Timer() {
    public void run() {
      // do some validation logic

      // tell the test system the test is now done
      finishTest();
    }
  };

  // Set a delay period significantly longer than the
  // event is expected to take.
  delayTestFinish(500);

  // Schedule the event and return control to the test system.
  timer.schedule(100);
}

```


> _Tip: The recommended pattern is to test one asynchronous event per test method. If you need to test multiple events in the same method, here are a couple of techniques:
    * "Chain" the events together. Trigger the first event during the test method's execution; when that event fires, call `delayTestFinish()` again with a new timeout and trigger the next event. When the last event fires, call `finishTest()` as normal.
    * Set a counter containing the number of events to wait for. As each event comes in, decrement the counter. Call `finishTest()` when the counter reaches `0.`_


### See Also ###

  * [GWTTestCase.delayTestFinish(int)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html#delayTestFinish(int))

  * [GWTTestCase.finishTest()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html#finishTest())
