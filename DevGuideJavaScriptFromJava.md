# Writing Native JavaScript Methods #

JSNI methods are declared `native` and contain JavaScript code in a specially formatted comment block between the end of the parameter list and the trailing semicolon. A JSNI comment block begins with the exact token `/*-{` and ends with the exact token `}-*/`. JSNI methods are called just like any normal Java method. They can be static or instance methods.

The JSNI syntax is a directive to the Java-to-JavaScript Compiler to accept any text between the comment statements as valid JS code and inject it inline in the generated GWT files.  At compile time, the GWT compiler performs some syntax checks on the JavaScript inside the method, then generates interface code for converting method arguments and return values properly.

As of the GWT 1.5 release, the Java varargs construct is supported.  The GWT compiler will translate varargs calls between 2 pieces of Java code.  However, calling a varargs JavaScript method from Java will result in the callee receiving the arguments in an array.


## Examples ##

Here is a simple example of how to code a JSNI method that puts up a JavaScript alert dialog:

```
public static native void alert(String msg) /*-{
  $wnd.alert(msg);
}-*/;
```


Note that the code did not reference the JavaScript `window` object directly inside the method. When accessing the browser's window and document objects from JSNI, you must reference them as `$wnd` and `$doc`, respectively.  Your compiled script runs in a nested frame, and `$wnd` and `$doc` are automatically initialized to correctly refer to the host page's window and document.

Here is another example with a problem:

```
public static native int badExample() /*-{
  return "Not A Number";
}-*/;
  
 public void onClick () {
   try {
      int myValue = badExample();
      GWT.log("Got value " + myValue, null);
   } catch (Exception e) {
      GWT.log("JSNI method badExample() threw an exception:", e);
   }
 }

```

This example compiles as Java, and its syntax is verified by the GWT compiler as valid JavaScript.  But when you run the example code in [hosted mode](DevGuideHostedMode.md), it returns an exception.  Click on the line in the hosted mode browser to display the exception in the message area below:

```
$PP_OFF
com.google.gwt.dev.shell.DevGuideHostedModeException: invokeNativeInteger(@com.example.client.GWTObjectNotifyTest::badExample()): JS value of type string, expected int
	at com.google.gwt.dev.shell.JsValueGlue.getIntRange(JsValueGlue.java:343)
	at com.google.gwt.dev.shell.JsValueGlue.get(JsValueGlue.java:179)
	at com.google.gwt.dev.shell.ModuleSpace.invokeNativeInt(ModuleSpace.java:233)
	at com.google.gwt.dev.shell.JavaScriptHost.invokeNativeInt(JavaScriptHost.java:97)
	at com.example.client.GWTObjectNotifyTest.badExample(GWTObjectNotifyTest.java:29)
	at com.example.client.GWTObjectNotifyTest$1.onClick(GWTObjectNotifyTest.java:52)
	...
```

In this case, neither the Java IDE nor the GWT compiler could tell that there was a type mismatch between the code inside the JSNI method and the Java declaration.  The GWT generated interface code caught the problem at runtime in hosted mode.  When running in [web mode](DevGuideWebMode.md), you will not see an exception.  JavaScript's dynamic typing obscures this kind of problem.

> _Tip: Since JSNI code is just regular JavaScript, you will not be able to use Java debugging tools inside your JSNI methods when running in hosted mode. However, you can set a breakpoint on the source line containing the opening brace of a JSNI method, allowing you to see invocation arguments. Also, the Java compiler and GWT compiler do not perform any syntax or semantic checks on JSNI code, so any errors in the JavaScript body of the method will not be seen until run time.
>_