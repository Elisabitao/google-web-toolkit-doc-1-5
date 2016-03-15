# Compatibility with the Java Language and Libraries #

## Language support ##

GWT supports most of the core Java language syntax and semantics, but there are a few differences you will want to be aware of.

> Note: As of GWT 1.5, GWT compiles the Java language syntax that is compatible with J2SE 1.5 or earlier. Versions of GWT prior to GWT 1.5 are limited to Java 1.4 source compatibility. For example, GWT 1.5 supports generics, whereas GWT 1.4 does not.

It is important to remember that the target language of your GWT application is ultimately JavaScript, so there are some differences between running your application in [hosted mode](DevGuideHostedMode.md) and [web mode](DevGuideWebMode.md):

  * **Intrinsic types**: Primitive types (`boolean`, `byte`, `char`, `short`, `int`, `long`, `float`, and `double`), `Object`, `String`, arrays, user-defined classes, etc. are all supported, with a couple of caveats.
    * **Arithmetic**: In JavaScript, the only available numeric type is a 64-bit floating point value.  All Java primitive numeric types (except for long, see below), are therefore implemented in web mode as if on doubles.  Primarily, that means overflowing an integral data type (`byte`, `char`, `short`, `int`) will not wrap the underlying value as Java specifies.  Instead, the resulting value will outside of the legal range for that data type.  Operations on `float` are performed as `double` and will result in more-than-expected precision.  Integer division is implemented to explicitly round to the correct value.
    * **`long`**: JavaScript has no 64-bit integral type, so `long` needs special consideration. Prior to GWT 1.5, the `long` type was was simply mapped to the integral range of a 64-bit JavaScript _floating-point_ value, giving `long` variables an actual  range less than the full 64 bits. As of GWT 1.5, `long` primitives are emulated as a pair of 32-bit integers, and work reliably over the entire 64-bit range.  Overflow is emulated to match the expected behavior.  There are a couple of caveats.  Heavy use of `long` operations will have a performance impact due to the underlying emulation.  Additionally, long primitives cannot be used in [JSNI code](DevGuideJavaScriptNativeInterface.md) because they are not a native JavaScript numeric type.

  * **Exceptions**: `try`, `catch`, `finally` and user-defined exceptions are supported as normal, although `Throwable.getStackTrace()` is not meaningfully supported in web mode.

> Note: Several fundamental exceptions implicitly produced by the Java VM, most notably `NullPointerException`, `StackOverflowError`, and `OutOfMemoryError`, do not occur in web mode as such. Instead, a `JavaScriptException` is produced for any implicitly generated exceptions.  This is because the nature of the underlying JavaScript exception cannot be reliably mapped onto the appropriate Java exception type.

  * **Assertions**:  `assert` statements are always active in hosted mode because it's a great way for GWT libraries to provide lots of helpful error checking while you're debugging. The GWT compiler removes and ignores all assertions by default, but you can enable them in web mode by specifying the `-ea` flag to `GWTCompiler`.

  * **Multithreading and Synchronization**:     JavaScript interpreters are single-threaded, so while GWT silently accepts the `synchronized` keyword, it has no real effect. Synchronization-related library methods are not available, including `Object.wait()`, `Object.notify()`, and `Object.notifyAll().`  The compiler will ignore the synchronized keyword but will refuse to compile your code if the `Object`'s related synchronization methods are invoked.

  * **Reflection**:    For maximum efficiency, GWT compiles your Java source into a monolithic script, and does not support subsequent dynamic loading of classes. This and other optimizations preclude general support for reflection. However, it is possible to query an object for its class name using Object.getClass().[getName()](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Class.html#getName()).

  * **Finalization**:    JavaScript does not support object finalization during garbage collection, so GWT is not able to be honor Java finalizers in web mode.

  * **Strict Floating-Point**:    The Java language specification precisely defines floating-point support, including single-precision and double-precision numbers as well as the `strictfp` keyword. GWT does not support the `strictfp` keyword and can not ensure any particular degree of floating-point precision in translated code, so you may want to avoid calculations in client-side code that require a guaranteed level of floating-point precision.

## Runtime library support ##

GWT supports only a small subset of the classes available in the Java 2 Standard and Enterprise Edition libraries, as these libraries are quite large and rely on functionality that is unavailable within web browsers. To find out exactly which classes and methods are supported for core Java runtime packages, see the GWT [JRE Emulation Reference](RefJreEmulation.md).


> _Tip: You will save yourself a lot of frustration if you make sure that you use only [translatable](DevGuideJavaToJavaScriptCompiler.md) classes in your [client-side code](DevGuideClientSide.md) from the very beginning. To help you identify problems early, your code is checked against the JRE emulation library whenever you run in [hosted mode](DevGuideHostedMode.md). As a result, most uses of unsupported libraries will be caught the first time you attempt to run your application. So,_run early and often_._

## Differences between JRE and emulated classes ##

Some specific areas in which GWT emulation differs from the standard Java runtime:

  * **Regular Expressions**:    The syntax of [Java regular expressions](http://java.sun.com/j2se/1.5.0/docs/api/java/util/regex/Pattern.html) is similar, but not identical, to [JavaScript regular expressions](http://developer.mozilla.org/en/docs/Core_JavaScript_1.5_Guide:Regular_Expressions). For example, the [replaceAll](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/String.html#replaceAll(java.lang.String,%20java.lang.String)) and [split](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/String.html#split(java.lang.String)) methods use regular expressions. So, you will probably want to be careful to only use Java regular expressions that have the same meaning in JavaScript.

  * **Serialization**:   Java serialization relies on a few mechanisms that are not available in compiled JavaScript, such as dynamic class loading and reflection. As a result, GWT does not support standard Java serialization. Instead, GWT has an [RPC](DevGuideRemoteProcedureCalls.md) facility that provides automatic object serialization to and from the server for the purpose of invoking remote methods.


> _Note: For a list of JRE classes that GWT can translate out of the box, see the GWT [JRE Emulation Reference](RefJreEmulation.md)._

## Classes that provide similar functionality ##

In some classes, the functionality of the class is too expensive to be emulated entirely, so a similar routine in another package is provided instead.  Here are some commonly used routines that provide a subset of the native JRE  functionality:

  * [com.google.gwt.i18n.client.DateTimeFormat](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/DateTimeFormat.html) : Supports a subset of `java.util.DateTimeFormat`.  See examples in the [date and number formatting](DevGuideDateAndNumberFormat.md) section.
  * [com.google.gwt.i18n.client.NumberFormat](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/NumberFormat.html) : Supports a subset of `java.util.NumberFormat`.  See examples in the [date and number format](DevGuideDateAndNumberFormat.md) section.
  * [com.google.gwt.user.client.rpc](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/IsSerializable.html) : A marker class used similarly to `java.io.Serializable` for GWT RPC
  * [com.google.gwt.user.client.Timer](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/Timer.html) : A simplified, browser-safe timer class. This class serves the same purpose as `java.util.Timer`, but is simplified because of the single-threaded environment.
