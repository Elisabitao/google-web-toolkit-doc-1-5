# Sharing objects between Java source and JavaScript #

Parameters and return types in JSNI methods are declared as Java types. There are very specific rules for how values passing in and out of JavaScript code must be treated. These rules must be followed whether the values enter and leave through normal method call semantics, or through the [special syntax](DevGuideJavaFromJavaScript.md).

## Passing Java values into JavaScript ##

|  **Incoming Java type** |  **How it appears to JavaScript code** |
|:------------------------|:---------------------------------------|
|   `String`              |   JavaScript string, as in  `var s = "my string";`   |
|   `boolean`             |  JavaScript boolean value, as in  `var b = true;`   |
|   `long`                |  disallowed  (see notes)               |
|   other numeric primitives  |  JavaScript numeric value, as in  `var x = 42;`   |
|   [JavaScriptObject](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JavaScriptObject.html) |  `JavaScriptObject` that must have originated from JavaScript code, typically as the return value of some other JSNI method  (see notes)  |
|   Java array            |  opaque value that can only be passed back into Java code |
|   any other Java `Object`   |  opaque value accessible through [special syntax](DevGuideJavaFromJavaScript.md)  |


## Passing JavaScript values into Java code ##

|  **Outgoing Java type** |  **What must be passed** |
|:------------------------|:-------------------------|
|   `String`              |  JavaScript string, as in  `return "boo";`   |
|   `boolean`             |  JavaScript boolean value, as in  `return false;`   |
|   `long`                |  disallowed (see notes)  |
|   Java numeric primitive  |  JavaScript numeric value, as in  `return 19;`   |
|    [JavaScriptObject](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JavaScriptObject.html) |   native JavaScript object, as in  `return document.createElement("div")`   (see notes)  |
|   any other Java `Object` (including arrays)  |  Java `Object` of the correct type that must have originated in Java code; Java objects cannot be constructed from "thin air" in JavaScript |


## Important Notes ##

  * The Java `long` type cannot be represented in JavaScript as a numeric type, so GWT emulates it using an opaque data structure.  This means that JSNI methods cannot process a `long` as a numeric type.  The compiler therefore disallows, by default, directly accessing a `long` from JSNI: JSNI methods cannot have `long` as a parameter type or a return type, and they cannot access a `long` using a [JSNI reference](DevGuideJavaFromJavaScript.md).  If you find yourself wanting to pass a `long` into or out of a JSNI method, here are some options:
    1. For numbers that fit into type `double`, use type `double` instead of type `long`.
    1. For computations that require the full `long` semantics, rearrange the code so that the computations happen in Java instead of in JavaScript.  That way they will use the `long` emulation.
    1. For values meant to be passed through unchanged to Java code, wrap the value in a `Long`.  There are no restrictions on type `Long` with JSNI methods.
    1. If you are sure you know what you are doing, you can add the annotation `com.google.gwt.core.client.UnsafeNativeLong` to the method.  The compiler will then allow you to pass a `long` into and out of JavaScript.  It will still be an opaque data type, however, so the only thing you will be able to do with it will be to pass it back to Java.

  * Violating any of these marshaling rules in [hosted mode](DevGuideHostedMode.md) will generate a `com.google.gwt.dev.shell.DevGuideHostedModeException` detailing the problem. This exception is not [translatable](DevGuideClientSide.md) and never thrown in [web mode](DevGuideWebMode.md).

  * [JavaScriptObject](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JavaScriptObject.html) gets special treatment from the GWT compiler and hosted browser. Its purpose is to provide an opaque representation of native JavaScript objects to Java code.

  * Although Java arrays are not directly usable in JavaScript, there are some helper classes that efficiently achieve a similar effect: [JsArray](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JsArray.html), [JsArrayBoolean](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JsArrayBoolean.html), [JsArrayInteger](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JsArrayInteger.html), [JsArrayNumber](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JsArrayNumber.html), and [JsArrayString](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JsArrayString.html).  These classes are wrappers around a native JavaScript array.

  * Java `null` and JavaScript `null` are identical and always legal values for any non-primitive Java type. JavaScript `undefined` is also considered equal to `null` when passed into Java code (the rules of JavaScript dictate that in JavaScript code, `null == undefined` is `true` but `null === undefined` is `false`).  In previous versions of GWT, `undefined` was not a legal value to pass into Java.