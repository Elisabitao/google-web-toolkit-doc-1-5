# Exceptions and JSNI #

An exception can be thrown during the execution of either normal Java code or the JavaScript code within a JSNI method.  When an exception generated within a JSNI method propagates up the call stack and is caught by a Java catch block, the thrown JavaScript exception is wrapped as a [JavaScriptException](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JavaScriptException.html) object at the time it is caught.  This wrapper object contains only the class name and description of the JavaScript exception that occurred.  The recommended practice is to handle JavaScript exceptions in JavaScript code and Java exceptions in Java code.

A Java exception can safely retain identity while propagating through a JSNI method.

For example,

  1. Java method `doFoo()` calls JSNI method `nativeFoo()`
  1. `nativeFoo()` internally calls Java method `fooImpl()`
  1. `fooImpl()` throws an exception

The exception thrown from `fooImpl()` will propagate through `nativeFoo()` and can be caught in `doFoo()`.  The exception will retain its type and identity.
