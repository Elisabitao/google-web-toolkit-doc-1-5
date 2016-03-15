# Glossary #

  * **asynchronous RPC** - A remote procedure call that returns control to the calling function immediately after making a request to the server.  A callback method will be called later to return the result (or an exception) from the server.  GWT-RPC methods are always asynchronous.

  * **composite widgets** - Widgets that are composed of one or more panels or widgets. Composite widgets are an easy way to [extend GWT's existing widgets](DevGuideCreatingCustomWidgets.md).

  * **cross-browser** - Refers to the ability to operate on multiple browser platforms (Internet Explorer, Mozilla, Safari, etc...). See the following [FAQ](FAQ_CurrentlySupportBrowsers.md) for the full list of browsers supported by GWT.

  * **deferred binding** - A technique used by the GWT compiler to create or select a specific implementation of a class based on a set of environment parameters.  The method [GWT.create()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class)) triggers [deferred binding](FAQ_DeferredBindingDefinition.md), allowing Java code to work with a declared Java interface. Some GWT features that use [deferred binding](DevGuideDeferredBinding.md) include [static string internationalization](DevGuideStaticStringInternationalization.md), [ImageBundle localization](DevGuideImageBundleWithLocalization.md) as well as [GWT-RPC](DevGuideRemoteProcedureCalls.md). For more details on deferred binding, check out the [deferred binding](DevGuideDeferredBinding.md) chapter in the development guide.

  * **deployment** - The act of copying your GWT compiled application files and public path contents to a web server, as well as porting your servlet implementations and configurations to a non-GWT servlet container.

  * **dynamic string internationalization** - Allows you to look up localized strings defined in a [host HTML](DevGuideHostPage.md) page at runtime using string-based keys.

  * **GWT compiler** - A part of the GWT toolkit that translates Java into JavaScript.

  * **host HTML page** - An HTML page that includes the generated `<module>.nocache.js` JavaScript in a `<script>` HTML tag.  This page may also contain HTML elements in its body, some of which your GWT module may reference or modify.

  * **hosted mode** - Refers to running your application under a special GWT supplied  browser.  Hosted mode runs your application directly in Java so that you can use a Java IDE debugger to help test and debug your application.

  * **module XML file** - A file that contains settings the GWT compiler and hosted mode use to find your projects resources, such as Java Source code, static HTML, stylesheets, and image files, and servlet classes.

  * **native methods** - Java methods that have a body implemented in JavaScript.  The GWT compiler creates interface code to the Java method's parameters and return values.

  * **overlay types** - A Java class that directly models a JavaScript object, giving you the development-time benefits of static types for JavaScript without adding any memory or speed costs at runtime.

  * **public path** - A directory or list of directories that contains static files that should be served by the web server.  All files in the static path are copied to the same directory as the compiled JavaScript output from the GWT compiler.  If no public path is specified in the [module XML file](DevGuideModuleXml.md), the default public path is `<module name>/public`.

  * **resource inclusion** - [A technique](DevGuideAutomaticResourceInjection.md) you can use in your [host HTML page](DevGuideHostPage.md) to reference external JavaScript and stylesheets.

  * **serializable types** - Types which are capable of being encoded and moved outside the application in order to be stored or transmitted to another application.  GWT has specific rules for [serializable types](DevGuideSerializableTypes.md) which must be used for RPC method parameters and return values.

  * **service proxy** - An implementation of an asynchronous interface in client-side code that links the GWT RPC  [RemoteServiceServlet](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/server/rpc/RemoteServiceServlet.html) implementation with your client code.  Service proxy interface names always have the same name as the server-side synchronous class and end with the suffix `Async`.

  * **source path** - A subpackage or list of subpackages that contain your module's GWT application code.  These subpackages are the ones that the GWT compiler will translate at compile time.  If no source path is specified in the [module XML file](DevGuideModuleXml.md), the default source path is `<module name>/client`.

  * **static string internationalization** - A technique of defining language specific strings in `.properties` files that GWT compiles into different implementations of your [Localizable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Localizable.html) subclasses.  Using deferred binding, the language strings will map to the correct locale at runtime.

  * **translatable** - Refers to Java code that can be translated from Java into JavaScript. In order to be translatable, code must only use the [supported subset](DevGuideJavaCompatibility.md) of the Java Runtime Library.

  * **web mode** - Refers to running the client-side of your application from a web browser instead of the hosted browser.  In web mode, your application runs from JavaScript generated by the GWT compiler.
