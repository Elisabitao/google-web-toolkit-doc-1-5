![http://google-web-toolkit-doc-1-5.googlecode.com/files/GWTbanner.png](http://google-web-toolkit-doc-1-5.googlecode.com/files/GWTbanner.png)

Writing web apps today is a tedious and error-prone process. Developers can spend 90% of their time working around browser quirks. In addition, building, reusing, and maintaining large JavaScript code bases and AJAX components can be difficult and fragile. Google Web Toolkit (GWT) eases this burden by allowing developers to quickly build and maintain complex yet highly performant JavaScript front-end applications in the Java programming language.

### How Google Web Toolkit works ###
With Google Web Toolkit, you write your AJAX front-end in the Java programming language using your favorite Java tools along the way. During development, use GWT's Hosted Mode browser to test and iterate in the same "tweak/refesh/view" cycle you're accustomed to, except that now you'll be able to easily step through and debug your web application. However, using GWT for your front-end doesn't mean you have to use Java for the backend. GWT provides various means of communicating with a backend server, including JSON. When you are ready to deploy, compile your Java source code to plain JavaScript using the GWT compiler, then deploy to any website or server. You can build one widget for an existing web page or an entire application in GWT.

![http://gwt-test-project-1.googlecode.com/files/GWTDiagram.png](http://gwt-test-project-1.googlecode.com/files/GWTDiagram.png)


### Compiler optimized JavaScript ###
The GWT compiler performs static analysis across your entire GWT codebase, optimizing the final JavaScript to load and execute faster than most hand-written JavaScript. For instance, it selectively inlines the bodies of methods, hoisting the content of methods into their original call sites, completely removing function call overhead and reducing the size of the final JavaScript to be downloaded.  This cross-compilation allows you to develop your application with maintainable abstractions and modularity without performance penalization.  [Learn more](DevGuideJavaToJavaScriptCompiler.md)


### Cross browser support for free ###
Your GWT applications automatically support IE, Firefox, Mozilla, Safari, and Opera with no browser detection or special-casing within your code. You write the same code once, GWT transforms it into the most performant JavaScript for the user's particular browser. [Learn more](DevGuideCrossBrowserSupport.md)


### Real debugging in GWT Hosted Mode ###
In production, your code is compiled to JavaScript, but at development time it runs in the Java virtual machine. That means when your code performs an action like handling a mouse event, you get full-featured Java debugging, with exceptions and the advanced debugging features of IDEs like [Eclipse](http://www.eclipse.org/). [Learn more](DevGuideHostedMode.md)

### Per-browser, per-locale, per-whatever optimized code ###
Deferred binding is a feature of the GWT compiler that works by generating many versions of code at compile time, only one of which needs to be loaded by a particular client during bootstrapping at runtime. Each version is generated on a per browser basis, along with any other axis that your application defines or uses. For example, if you were to internationalize your application using GWT's [Internationalization module](DevGuideInternationalization.md), the GWT compiler would generate versions of your application per browser environment, such as "Firefox in English", "Firefox in French", "Internet Explorer in English", etc... As a result, the deployed JavaScript code is compact and quicker to download than if you coded if/then statements into your hand-written JavaScript, containing only the code and resources it needs for a particular browser environment. [Learn more](DevGuideDeferredBinding.md)


### Dynamic, reusable UI components ###
Create reusable [Widgets](DevGuideUserInterface.md) by compositing other Widgets, then easily lay them out automatically in [Panels](DevGuideUserInterface.md). The [GWT Showcase](SampleShowcase.md) application provides an overview of the various UI features in GWT. Want to reuse your Widget in another project? Simple package it up for others to use in a JAR file. [Learn more](DevGuideUserInterface.md)


### Productive coding and tools ###
Because GWT uses Java, you can use all of your favorite Java development tools ([Eclipse](http://www.eclipse.org/), [IntelliJ](http://www.jetbrains.com/idea/), [JProfiler](http://www.ej-technologies.com/products/jprofiler/overview.html), [JUnit](http://www.junit.org/)) for your AJAX development. This allows a web developer to harness the productivity gains of automated Java refactoring and code prompting/completion. In addition, static type checking in the Java language enables developers to catch a class of JavaScript bugs (typos, type mismatches) when writing code rather than at runtime, boosting productivity while reducing errors. No more user discovered accidental var assignments. Finally, you can take advantage of Java-based OO designs patterns and abstractions that are easy to understand and maintain without your user incurring any runtime performance costs thanks to the compiler optimizations.


### Really simple RPC ###
To communicate from your web application to your web server, you just need to define serializable Java classes for your request and response. In production, GWT automatically serializes the request and deserializes the response from the server. GWT's RPC mechanism can even handle polymorphic class hierarchies, and you can throw exceptions across the wire. GWT also supports other transfer protocols, such as JSON. [Learn more](DevGuideRemoteProcedureCalls.md)


### Interoperability with native JavaScript and other libraries ###
If GWT's class library doesn't meet your needs, you can mix handwritten JavaScript in your Java source code using our [JavaScript Native Interface (JSNI)](DevGuideJavaScriptNativeInterface.md). With GWT 1.5, it is now possible to subclass the GWT JavaScriptObject (JSO) class to create Java "class overlays" onto arbitrary JavaScript objects. Thus, you can get the benefits of modeling JS objects as proper Java types (e.g. code completion, refactoring, inlining) without any additional memory or speed overhead. This capability makes it possible to use JSON structures optimally. [Learn more](DevGuideJavaScriptNativeInterface.md)


### Browser history management ###
No, AJAX applications _don't_ need to break the browser's back button. GWT lets you make your site more usable by easily adding state to the browser's back button history. [Learn more](DevGuideHistory.md)


### JUnit integration ###
GWT's direct integration with [JUnit](http://junit.org/) lets you unit test both in a debugger and in a browser...and you can even unit test asynchronous RPCs. [Learn more](DevGuideJUnitIntegration.md)


### Internationalization ###
Easily create efficient internationalized applications and libraries using GWT's powerful Deferred Binding techniques. In addition, as of 1.5 the standard GWT widgets support bidirectionality (see the showcase example). [Learn more](DevGuideInternationalization.md)


### Completely Open Source ###
All of the code for GWT is available under the Apache 2.0 license. If you are interested in contributing, please visit [Making GWT Better](http://code.google.com/webtoolkit/makinggwtbetter.html). [Learn more](http://code.google.com/p/google-web-toolkit/)


### Next steps ###
Ready to get started? For a step-by-step installation guide and tutorial, please see the [Getting Started Guide](GettingStarted.md).
