# Deploy #

## Web Mode ##

So far, we've been running StockWatcher in [hosted mode](DevGuideHostedMode.md) to allow for easy debugging.  Now that we're ready to deploy the application, we'll use the GWT compiler to actually compile our Java source code to JavaScript.  When StockWatcher is compiled, it is running in what's called _web mode_.  When an application runs in web mode, it exists as pure JavaScript and does not require any browser plug-ins or the Java Virtual Machine (JVM).

## Compiling StockWatcher ##

There are a couple of different ways you can compile StockWatcher.  The first is to use the _Compile/Browse_ button in the hosted mode browser.  Go ahead and launch StockWatcher now and click _Compile/Browse_.

After the application is compiled, it should appear in a new browser window looking just as it did in hosted mode.  The real difference is hidden under the covers.  When you interact with StockWatcher now, it's executing JavaScript code in the browser, not Java bytecode in the JVM.

_On Linux, in order to make your default browser open when you click_Compile/Browse_you'll need to first set the `GWT_EXTERNAL_BROWSER` environment variable in your `StockWatcher-shell` script and/or your IDE launch configuration to point to your browser executable_.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherWebMode.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherWebMode.png)

If you look at the Development Shell window, you'll notice new log entries indicating the result of the compilation:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDevShellCompile.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDevShellCompile.png)

The second log message tells us where the GWT compiler placed the compiled JavaScript version of StockWatcher.  Its path will end in: `com.google.gwt.sample.stockwatcher.StockWatcher`, which corresponds to the name of the StockWatcher [module](DevGuideModules.md).  The compiler will create one output directory for each module.

## Compiler output ##

If you browse the output directory for StockWatcher, you should see a set of files similar to these:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherWebModeFiles.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherWebModeFiles.png)

The first thing you'll probably notice are all the filenames containing GUID's.  These files contain the actual JavaScript implementations of StockWatcher.  Notice the use of the plural form: _implementations_.  This is because GWT actually generates unique implementations of your application for each supported web browser.  At runtime, GWT uses a mechanism called [deferred binding](FAQ_DeferredBindingDefinition.md) to load the correct version for the end user's browser.  In this way, it can seamlessly work around the bugs and idiosyncrasies of the various web browsers with no additional work on your part, and no unnecessary bytes in your application download.

Besides browser detection, deferred binding can also generate customized versions of your application for any number of other variables.  One very common example is internationalization.  With deferred binding, GWT generates a separate implementation of the application for each language, so for example, an English speaker doesn't have to download the French text (and vice versa).  If you're interested, we'll actually be  [internationalizing StockWatcher](GettingStartedI18n.md) later in the Getting Started guide.

In the output directory you'll also notice our project's static resources, including the `StockWatcher.html` [host page](DevGuideHostPage.md), the `StockWatcher.css` file, the `images` subdirectory containing the Google Code logo, and the `gwt` subdirectory containing the files used by the GWT standard visual theme.

## Deploying StockWatcher ##

At this point, we could deploy StockWatcher to a public web server simply by uploading the files in the output directory.  Because this initial version does not need to [communicate with the server](GettingStartedRPC.md) in any way, it does not require anything special on the part of the web server: any server that can serve up static web pages will do just fine.

## Project compilation script ##

There's another way to compile our GWT applications.  If you think back to when we first [created](GettingStartedCreateProject.md) the StockWatcher project, you may recall that the [applicationCreator](DevGuideApplicationCreator.md) utility generated a script named `StockWatcher-compile` in the main project directory.  That script is designed to invoke the GWT compiler just as the _Compile/Browse_ button in the Development Shell window does.

If you look inside the script, you'll see that it wraps a call to the Java class `com.google.gwt.dev.GWTCompiler`, which is where the GWT compiler actually lives.  There are several [command-line options](DevGuideModuleCompileScript.md) you can pass to the script to modify the compiler output.

## Next steps ##

By now you should have a pretty good idea of how to develop a GWT application, from start to finish.  If you're interested in learning additional features of GWT you can  follow along in the [next section](GettingStartedMore.md) of the Getting Started guide as we add remote procedure calls, internationalization, and unit tests to our StockWatcher sample.
