# How do I pass a Java method as a callback function? #

It is quite common for a JavaScript api to return a value asynchronously through a callback function.   You can refer to Java functions as first-class objects with [JSNI](DevGuideJavaScriptNativeInterface.md)
syntax.   Assuming a JavaScript function `externalJsFunction` that takes a data value and a callback function, here is an example of how to code this:

```
package p;

class C {
  void doCallback(String callbackData) { ..... }
  native void invokeExternal(String data) /*-{
    $wnd.externalJsFunction(data, @p.C::doCallback(Ljava/lang/String;));
  }-*/;
}
```

Depending on the nature of the callback, it's sometimes
helpful to use an anonymous JavaScript function to create a wrapper callback when you invoke the API method.  When the callback is invoked, wrapper will forward the parameter values to the Java method:

```
package p;

class D {
  void someCallback(int param1, int param2, String param3) { ..... }
  native void invokeExternal(String data) /*-{
    $wnd.externalJsFunction(data, function(int1, int2, string3) {
      @p.D::someCallback(IILjava/lang/String;)(int1, int2, string3);
    });
  }-*/
}
```
