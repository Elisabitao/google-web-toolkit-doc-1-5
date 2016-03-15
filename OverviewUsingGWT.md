# Using GWT #

You can use GWT's set of UI components (called [Widgets](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Widget.html)) to construct the UI elements that make up your AJAX application. Like traditional UI frameworks, Widgets are combined in [Panels](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Panel.html) that determine the layout of the widgets contained within them. This is a complete GWT application that displays a button with a click handler:

```
public class Hello implements EntryPoint {
  public void onModuleLoad() {
    Button b = new Button("Click me", new ClickListener() {
      public void onClick(Widget sender) {
        Window.alert("Hello, AJAX");
      }
    });
    RootPanel.get().add(b);
  }
}
```

GWT supports a variety of built-in Widgets that are useful for AJAX applications, including [hierarchical trees](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Tree.html), [tab bars](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TabBar.html), [menu bars](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/MenuBar.html), and [modal dialog boxes](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/DisclosurePanel.html). GWT also has built-in support for remote procedure calls and other more sophisticated web application features. See the [complete list of features](OverviewFeatures.md) for more information.

## Debugging and Deploying GWT Applications ##

GWT applications can be run in two modes:

  * **Hosted mode** - In hosted mode, your application is run as Java bytecode within the Java Virtual Machine (JVM). You will typically spend most of your development time in hosted mode because running in the JVM means you can take advantage of Java's debugging facilities and remain within an IDE like [Eclipse](http://www.eclipse.org/).

  * **Web mode** - In web mode, your application is run as pure JavaScript and HTML, compiled from your original Java source code with the GWT Java-to-JavaScript compiler. When you deploy your GWT applications to production, you deploy this JavaScript and HTML to your web servers, so end users will only see the web mode version of your application.

To support hosted mode, GWT ships with a special web browser with hooks into the JVM. See the GWT architecture diagram below for more information.

For a step-by-step installation and usage guide, please see the [Getting Started Guide](GettingStarted.md).

## Google Web Toolkit Architecture ##

GWT has four major components: a Java-to-JavaScript compiler, a "hosted" web browser, and two Java class libraries:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/ArchitectureDiagram.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/ArchitectureDiagram.png)

The components, from bottom to top, are:

  * **GWT Java-to-JavaScript Compiler** - The GWT [Java-to-JavaScript compiler](DevGuideJavaToJavaScriptCompiler.md) translates the [Java](http://java.sun.com/) programming language to the [JavaScript](http://www.ecma-international.org/publications/standards/Ecma-262.htm) programming language. You use the GWT compiler to run your GWT applications in [web mode](DevGuideWebMode.md).

  * **GWT Hosted Web Browser** - The GWT Hosted Web Browser lets you run and execute your GWT applications in [hosted mode](DevGuideHostedMode.md), where your code runs as Java in the Java Virtual Machine without compiling to JavaScript. To accomplish this, the GWT browser embeds a special browser control (an Internet Explorer control on Windows or a Gecko/Mozilla control on Linux) with hooks into the JVM.

  * **JRE emulation library** - GWT contains JavaScript implementations of the most widely used classes in the Java standard class library, including most of the `java.lang` package classes and a subset of the `java.util` package classes. The rest of the Java standard library isn't supported natively within GWT. For example, packages like `java.io` don't apply to web applications since they access the network and local file system.

  * **GWT Web UI class library** - The GWT [web UI class library](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/package-summary.html) is a set of custom interfaces and classes that let your create web browser "widgets," like buttons, text boxes, images, and text. This is the core user interface library used to create GWT applications.

For a step-by-step installation and usage guide, please see the [Getting Started Guide](GettingStarted.md).

If you're curious about the underlying source code, please feel free to [check it out](http://code.google.com/p/google-web-toolkit/). The source code for all of GWT is available under the [Apache 2.0 license](http://code.google.com/webtoolkit/terms.html).
