# Design the application #

Now that we've generated our skeleton GWT project using [projectCreator](DevGuideProjectCreator.md) and [applicationCreator](DevGuideApplicationCreator.md), let's start designing our application. StockWatcher is going to be a very simple stock watch list.  Here's what it should end up looking like when we're finished:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcher.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcher.png)

Let's talk about the features we're going to need:

## User interface ##

### Stock List ###

The stocks the user is watching will be displayed in a table. We'll want to display the stock's symbol, along with the current price and its price change today (expressed as both the absolute change and the percentage change). Since this is an AJAX application, we don't want to make the user click the browser's refresh button to update the prices. Instead, we'll automatically refresh the list periodically. Each time we do, we'll display the time of the last update to reassure the user that the prices displayed are current.

Since StockWatcher will initially run entirely from the client-side, we'll just randomly generate the stock price data. Thus, you probably shouldn't make investment decisions based on the prices you see in StockWatcher. We'd recommend using something like [Google Finance](http://finance.google.com/) instead.

### Adding/Removing Stocks ###

The user needs to be able to add stocks to his list. He'll add a stock by entering its symbol into a textbox and clicking an Add button. The application should first check the symbol to make sure it's valid. For our example, we'll verify that the stock symbol is a string of between 1 and 5 alpha-numeric characters. If the user tries to add a symbol already in the watch list, we'll ignore the request.

We'll add another column that contains a button to remove each stock. When the user clicks the button, the stock will be immediately removed from the list.

## Client-side code ##

The core of GWT is a compiler that converts Java source into JavaScript, transforming your working Java application into an equivalent JavaScript application.  However, because of limitations of the JavaScript language and the web browser environment in which it runs, there are certain aspects of Java that GWT does not support.  We'll need to keep those in mind as we write the source code for StockWatcher.

### Language support ###

GWT supports most Java 1.5 language semantics.  You can use any of the intrinsic types as well as custom classes in your applications.  However, JavaScript does not support 64-bit integers or guaranteed floating-point precision, so you should avoid using `long` variables or the `strictfp` keyword in your Java source code.

JavaScript (and by extension, GWT) does not support multithreading, so be sure not to use the `synchronized` keyword or try to call the methods `Object.wait()`, `Object.notify()`, or `Object.notifyAll()`.

Also, GWT does not support reflection, although it is possible to query an object for its class name using Object.getClass().[getName()](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Class.html#getName()).

Finally, although both JavaScript and Java use garbage collection, JavaScript doesn't support object finalization so any Java finalizers you define will not be executed when your application runs in [web mode](GettingStartedWebMode.md).

The Developer's Guide has more details on [Java language support](DevGuideJavaCompatibility.md).

### Runtime library support ###

The Java 2 Standard and Enterprise Edition libraries are quite large and many classes rely on functionality not available within the web browser.  Therefore, GWT supports only a subset of the Java platform contained in the `java.lang` and `java.util` packages.  The emulated GWT versions of these classes will generally behave just as they do in regular Java, with a few notable exceptions.  For example, the syntax of [Java regular expressions](http://java.sun.com/j2se/1.4.2/docs/api/java/util/regex/Pattern.html) is not _quite_ the same as [JavaScript regular expressions](http://developer.mozilla.org/en/docs/Core_JavaScript_1.5_Guide:Regular_Expressions), so when calling methods that use regular expressions (e.g., String's [replaceAll(String,String)](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/String.html#replaceAll(java.lang.String,%20java.lang.String)) and [split(String)](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/String.html#split(java.lang.String)) methods), make sure you only use regular expressions that have the same meaning in both Java and JavaScript.

You can find out more about [Java runtime library support](DevGuideJavaCompatibility.md) in the Developer Guide.

### Ensuring GWT compatibility ###

As you write your application, be sure to only use language features and library classes in your Java source code that are supported by GWT.  To help you find incompatibilities early in the development process, your code is verified against the JRE emulation library when you run in [hosted mode](DevGuideHostedMode.md), thus catching most uses of unsupported Java functionality the first time you try to run your application.
