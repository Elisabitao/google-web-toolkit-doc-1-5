# How does GWT work? #
With Google Web Toolkit (GWT), you write your AJAX front-end in the Java programming language using the Java development tools of your choice. If GWT's class library doesn't meet your needs, you can mix handwritten JavaScript in your Java source code using the [JavaScript Native Interface (JSNI)](DevGuideJavaScriptNativeInterface.md). GWT then cross-compiles this client-side code into optimized JavaScript that automatically works across all major browsers.

Using GWT you can build an entire web application from scratch or build single widgets and integrate them into existing web pages.

GWT ships with a special _hosted mode_ web browser to use while developing and debugging. You can iterate quickly in the same "edit - refesh - view" cycle you're accustomed to with JavaScript, with the added benefit of being able to debug and step through your Java code line by line. In production, your code is compiled to JavaScript, but in hosted mode it runs in the Java virtual machine. That means when your code performs an action like handling a mouse event, you get full-featured Java debugging, with exceptions and the advanced debugging features of IDEs like [Eclipse](http://www.eclipse.org/).

GWT RPC provides a mechanism that simplifies communication from your web application to your web server. You define serializable Java classes for your request and response. In production, GWT automatically serializes the request and deserializes the response from the server. GWT's RPC mechanism can even handle polymorphic class hierarchies, and you can throw exceptions across the wire.

If you aren't running Java on back end, you can use GWT to retrieve JSON or XML.

GWT's direct integration with [JUnit](http://junit.org/) lets you unit test both in a debugger and in a browser...and you can even unit test asynchronous RPCs.

When you deploy your application to production, the GWT compiler translates your Java application to browser-compliant JavaScript and HTML.
