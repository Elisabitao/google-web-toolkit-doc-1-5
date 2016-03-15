# How do I call Java methods from handwritten JavaScript or third party libraries? #

You can define a _bridge_ method via JSNI that provides an external, globally visible JavaScript function that can be called by your hand-crafted JavaScript code. For example,

```
package mypackage;
public MyUtilityClass
{
    public static int computeLoanInterest(int amt, float interestRate, int term) { ... }
    public static native void defineBridgeMethod() /*-{
       $wnd.computeLoanInterest = function(amt, intRate, term) {
          return @mypackage.MyUtilityClass::computeLoanInterest(IFI)(amt, intRate, term);
       }
    }-*/;
}
```

On application initialization, call `MyUtilityClass.defineBridgeMethod()` (e.g. from your GWT Entry Point). This will create a function on the window object called `computeLoanInterest()` which will invoke, via JSNI, the compiled Java method of the same name. The bridge method is needed because the GWT compiler will obfuscate/compress/rename the names of Java methods when it translates them to JavaScript.