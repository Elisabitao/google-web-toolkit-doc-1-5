# Accessing Java Methods and Fields from JavaScript #

It can be very useful to manipulate Java objects from within the JavaScript implementation of a JSNI method. However, since JavaScript uses dynamic typing and Java uses static typing, you must use a special syntax.

> _When writing JSNI code, it is helpful to occasionally run in [web mode](DevGuideWebMode.md). The [JavaScript compiler](DevGuideJavaToJavaScriptCompiler.md) checks your JSNI code and can flag errors at compile time that you would not catch until runtime in [hosted mode](DevGuideHostedMode.md)._

## Invoking Java methods from JavaScript ##

Calling Java methods from JavaScript is somewhat similar to calling Java methods from C code in [JNI](http://java.sun.com/j2se/1.4.2/docs/guide/jni/index.html). In particular, JSNI borrows the JNI mangled method signature approach to distinguish among overloaded methods.
JavaScript calls into Java methods are of the following form:

```
$PP_OFF
[instance-expr.]@class-name::method-name(param-signature)(arguments)
```

  * **[instance-expr.]** : must be present when calling an instance method and must be absent when calling a static method
  * **class-name** :  is the fully-qualified name of the class in which the method is declared (or a subclass thereof)
  * **param-signature** : is the internal Java method signature as specified [here](http://java.sun.com/j2se/1.4.2/docs/guide/jni/spec/types.html#wp16432) but without the trailing signature of the method return type since it is not needed to choose the overload
  * **arguments** : is the actual argument list to pass to the called method

## Invoking Java constructors from JavaScript ##

Calling Java constructors from JavaScript is identical to the above use case, except that the method name is alway **new**.

Given the following Java classes:
```
package pkg;
class TopLevel {
  public TopLevel() { ... }
  public TopLevel(int i) { ... }

  static class StaticInner {
    public StaticInner() { ... }
  }

  class InstanceInner {
    public InstanceInner(int i) { ... }
  }
}
```

We compare the Java expression versus the JSNI expression:
  * `new TopLevel()` becomes `@pkg.TopLevel::new()()`
  * `new StaticInner()` becomes `@pkg.TopLevel.StaticInner::new()()`
  * `someTopLevelInstance.new InstanceInner(123)` becomes `@pkg.TopLevel.InstanceInner::new(Lpkg/TopLevel;I)(someTopLevelInstance, 123)`
    * The enclosing instance of a non-static class is implicitly defined as the first parameter for constructors of a non-static class.  Regardless of how deeply-nested a non-static class is, it only needs a reference to an instance of its immediately-enclosing type.

## Accessing Java fields from JavaScript ##

Static and instance fields can be accessed from handwritten JavaScript. Field references are of the form
```
$PP_OFF
[instance-expr.]@class-name::field-name
```

## Example ##

```
public class JSNIExample {

  String myInstanceField;
  static int myStaticField;

  void instanceFoo(String s) {
    // use s
  }

  static void staticFoo(String s) {
    // use s
  }

  public native void bar(JSNIExample x, String s) /*-{
    // Call instance method instanceFoo() on this
    this.@com.google.gwt.examples.JSNIExample::instanceFoo(Ljava/lang/String;)(s);

    // Call instance method instanceFoo() on x
    x.@com.google.gwt.examples.JSNIExample::instanceFoo(Ljava/lang/String;)(s);

    // Call static method staticFoo()
    @com.google.gwt.examples.JSNIExample::staticFoo(Ljava/lang/String;)(s);

    // Read instance field on this
    var val = this.@com.google.gwt.examples.JSNIExample::myInstanceField;

    // Write instance field on x
    x.@com.google.gwt.examples.JSNIExample::myInstanceField = val + " and stuff";

    // Read static field (no qualifier)
    @com.google.gwt.examples.JSNIExample::myStaticField = val + " and stuff";
  }-*/;

}
```

> _Tip: As of the GWT 1.5 release, the Java varargs construct is supported.  The GWT compiler will translate varargs calls between two pieces of Java code, however, calling a varargs Java method from JSNI will require the JavaScript caller to pass an array of the appropriate type.
>_

## Calling a Java Method from Handwritten JavaScript ##

Sometimes you need to access a method or constructor defined in GWT from outside JavaScript code.  This code might be hand-written and included in another java script file, or it could be a part of a third party library.  In this case, the GWT compiler will not get a chance to build an interface between your JavaScript code and the GWT generated JavaScript directly.

A way to make this kind of relationship work is to assign the method via JSNI to an external, globally visible JavaScript name that can be referenced by your hand-crafted JavaScript code.

```
package mypackage;

public MyUtilityClass
{
    public static int computeLoanInterest(int amt, float interestRate, 
                                          int term) { ... }
    public static native void exportStaticMethod() /*-{
       $wnd.computeLoanInterest =
          @mypackage.MyUtilityClass::computeLoanInterest(IFI);
    }-*/;
}
```

On application initialization, call `MyUtilityClass.exportStaticMethod()` (e.g. from your GWT Entry Point). This will assign the function to a variable in the window object called `computeLoanInterest`.
