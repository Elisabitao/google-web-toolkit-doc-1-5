# JavaScript Native Interface (JSNI) #

The [GWT compiler](DevGuideJavaToJavaScriptCompiler.md) translates Java source into JavaScript. Sometimes it's very useful to mix handwritten JavaScript into your Java source code. For example, the lowest-level functionality of certain core GWT classes are handwritten in JavaScript. GWT borrows from the Java Native Interface (JNI) concept to implement JavaScript Native Interface (JSNI).  Writing JSNI methods is a powerful technique, but should be used sparingly because writing bulletproof JavaScript code is notoriously tricky. JSNI code is potentially less portable across browsers, more likely to leak memory, less amenable to Java tools, and harder for the compiler to optimize.

We think of JSNI as the web equivalent of inline assembly code. You can use it in many ways:

  * Implement a Java method directly in JavaScript
  * Wrap type-safe Java method signatures around existing JavaScript
  * Call from JavaScript code into Java code and vice-versa
  * Throw exceptions across Java/JavaScript boundaries
  * Read and write Java fields from JavaScript
  * Use hosted mode to debug both Java source (with a Java debugger) and JavaScript (with a script debugger, only in Windows right now)


## Topics ##
[Writing Native JavaScript Methods](DevGuideJavaScriptFromJava.md)
Declare a `native` method and write the body into a specially formatted comment.

[Accessing Java Methods and Fields from JavaScript](DevGuideJavaFromJavaScript.md)
Handwritten JavaScript can invoke methods and access fields on Java objects.

[Sharing objects between Java source and JavaScript](DevGuideMarshaling.md)
How Java objects appear to JavaScript code and vice-versa.

[Exceptions and JSNI](DevGuideJsniExceptions.md)
How JavaScript exceptions interact with Java exceptions and vice-versa.

[JavaScript Overlay Types](DevGuideOverlayTypes.md)
How to easily integrate JavaScript objects into your GWT project and interact directly with JavaScript objects from your Java source code.