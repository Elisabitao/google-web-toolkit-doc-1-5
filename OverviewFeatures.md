# GWT Features #

  * [Dynamic, reusable UI components](DevGuideUserInterface.md) - Create [Widgets](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Widget.html) by compositing other Widgets. Lay out Widgets automatically in [Panels](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Panel.html). Send your Widget to other developers in a JAR file.

  * [Really simple RPC](DevGuideRemoteProcedureCalls.md) - To communicate from your web application to your web server, you just need to define serializable Java classes for your request and response. In production, GWT automatically serializes the request and deserializes the response from the server. GWT's RPC mechanism can even handle polymorphic class hierarchies, and you can throw exceptions across the wire.

  * [Browser history management](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/History.html) - No, AJAX applications _don't_ need to break the browser's back button. GWT lets you make your site more usable by easily adding state to the browser's back button history.

  * [Real debugging](DevGuideHostedMode.md) - In production, your code is compiled to JavaScript, but at development time it runs in the Java virtual machine. That means when your code performs an action like handling a mouse event, you get full-featured Java debugging, with exceptions and the advanced debugging features of IDEs like [Eclipse](http://www.eclipse.org/).

  * [Browser compatible](DevGuideCrossBrowserSupport.md) - Your GWT applications automatically support IE, Firefox, Mozilla, Safari, and Opera with no browser detection or special-casing within your code in most cases.

  * [JUnit integration](DevGuideJUnitIntegration.md) - GWT's direct integration with [JUnit](http://junit.org/) lets you unit test both in a debugger and in a browser...and you can even unit test asynchronous RPCs.

  * [Internationalization](DevGuideInternationalization.md) - Easily create efficient internationalized applications and libraries.

  * [Interoperability and fine-grained control](DevGuideJavaScriptNativeInterface.md) - If GWT's class library doesn't meet your needs, you can mix handwritten JavaScript in your Java source code using our [JavaScript Native Interface (JSNI)](DevGuideJavaScriptNativeInterface.md).

  * [Google API Library: Google Gears support](http://code.google.com/p/gwt-google-apis/) - We are in the process of building support for using Google APIs in GWT applications. Initially, we are providing support for [Google Gears](http://code.google.com/apis/gears/), the recently-launched developer product that extends the browser to allow developers to make web-based applications function even while offline. If you would like to download this library please visit the [open source project](http://code.google.com/p/gwt-google-apis/). We are planning to add support for other [Google APIs](http://code.google.com/apis/); if you'd like to help, please check out [Making GWT Better](http://code.google.com/webtoolkit/makinggwtbetter.html).

  * [Completely Open Source](http://code.google.com/p/google-web-toolkit/) - All of the code for GWT is available under the Apache 2.0 license. If you are interested in contributing, please visit [Making GWT Better](http://code.google.com/webtoolkit/makinggwtbetter.html).

For a step-by-step installation and usage guide, please see the [Getting Started Guide](GettingStarted.md).
