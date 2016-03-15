# Debug in hosted mode #

If you're reading the Getting Started guide in order, you'll remember the cliff-hanger ending to our [last section](GettingStartedClientFunc.md): where's the bug in StockWatcher?

## Fixing StockWatcher ##

If you're stumped, take another look at a snapshot of our current iteration of StockWatcher:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherPriceUpdates.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherPriceUpdates.png)

Notice anything odd about the change percentages?  Don't they seem a bit small?  If you do the math, you'll discover that they appear to be exactly an order of magnitude smaller than they should be.  There's an arithmetic error hiding somewhere in StockWatcher's code.  Thanks to GWT, though, it shouldn't be too difficult to find.

Open up the StockWatcher project in Eclipse.  We'll start our investigation in the updateTable(StockPrice) function since that's where we're updating the change percent text in the FlexTable.  Let's go ahead and set a breakpoint (_Ctrl-Shift-B_ or _Run -> Toggle Breakpoint_) on the following line of code:

```
String changePercentText = changeFormat.format(price.getChangePercent());
```

Now debug StockWatcher by clicking the _Debug_ button on the toolbar or selecting the _Run -> Debug_ menu item.  Eclipse will launch StockWatcher in hosted mode.  To run the code in updateTable(StockPrice), add one stock symbol to the watch list.  When you do, execution will stop at our breakpoint in the Eclipse window.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDebug1.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDebug1.png)

We can see that the parameter to the call to changeFormat.format(double) is coming from the getChangePercent() member of our StockPrice object.  Let's drill down to see what value is getting calculated there.  Step into the getChangePercent() call by hitting _F5_ or selecting the _Run -> Step Into_ menu item.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDebug2.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDebug2.png)

Now we're inside getChangePercent().  The one line of code there instantly reveals the problem: we're multiplying the change percentage by 10 instead of 100.  That corresponds exactly with the output we were seeing before: all of the change percentages were only 1/10 the size of the correct values.

Since we've located the source of the bug, let's stop debugging by selecting _Run -> Terminate_.  Fix the broken getChangePercent() method with the following code:

```
public double getChangePercent() {
  return 100.0 * this.change / this.price;
}
```

Don't forget to test the change to verify the fix!  Launch StockWatcher again and check the change percentages it calculates now:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDebug3.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDebug3.png)

Perfect!  Now we're getting the right values and StockWatcher is working just as expected.

## More about hosted mode ##

Before we continue building our application, let's talk a little more about hosted mode.  There are a couple of important points that you should know.

### Refreshing hosted mode ###

You do not always need to restart hosted mode after modifying your source code.  Instead, just click the _Refresh_ button in the hosted mode browser after saving your changes, and hosted mode will automatically recompile your application and open the new version.

You may have noticed that sometimes your changes take effect even if you _do not_ refresh hosted mode.  This behavior is a result of the way hosted mode interacts with the compiled code, but it is not always reliable.  Specifically, it only happens when you make minor changes to existing functions, such as the simple calculation change we made above.  Make it a habit to always refresh the hosted mode browser after making changes to ensure your changes are included.

### GWTShell ###

Thus far, we haven't really discussed exactly what's happening behind the scenes when you launch your GWT application in hosted mode, either by running the generated script (e.g., `StockWatcher-shell`) or by running/debugging the application from Eclipse.

The magic of hosted mode lives in the hosted mode startup class, `com.google.com.gwt.dev.GWTShell`, found in `gwt-dev-windows.jar` (or `gwt-dev-linux.jar`).  If you look inside the `StockWatcher-shell` script, you'll find `GWTShell` followed by a couple of its important command line arguments.

The first of these is the `-out` parameter that specifies the directory to write output files into.  In `StockWatcher-shell`, this is set to the `www` subdirectory.

The other parameter (the one at the end with no flag preceding it) is the `url` parameter.  This value tells `GWTShell` which URL to launch in the [hosted mode browser](DevGuideHostedMode.md) when it starts up.  For StockWatcher, this address is `localhost:8888/com.google.gwt.sample.stockwatcher.StockWatcher/StockWatcher.html`.

This, of course, leads to the question: what's serving up web pages from my computer on port 8888?  The answer is: an embedded [Tomcat](http://tomcat.apache.org/) server.  By default, the `GWTShell` utility hosts an instance of Tomcat that provides a servlet container for your GWT application.  Why would you need that?  Primarily, you'll use the embedded Tomcat instance for testing the implementation of your backend services.  Without getting too deep into the details, GWT provides classes that make it easy for your application to pass Java objects to and from a servlet-based service over HTTP.  We'll talk more about that exciting subject later, in the section on using GWT [remote procedure calls](GettingStartedRPC.md).

### Using another web server ###

Now that you know about the embedded Tomcat server, you might wonder whether it's possible to use a different web server in hosted mode.  Absolutely!  By changing just a couple of the parameters to `GWTShell`, we can serve hosted mode GWT applications with any web server we like.  The application will still execute as Java bytecode, so you'll  be able to debug your application just as you normally would.

To use another web server, you'll need to first disable the embedded Tomcat server.  This can be accomplished by passing the `-noserver` flag to the `GWTShell` utility.  Then, modify the `url` parameter to point to the address where your web server is configured to serve your GWT application from.  Finally, add a `-port` flag followed by your server's port number (otherwise, `GWTShell` will use the default port 8888).

Let's illustrate this with a real example.

When we discussed adding GWT to an existing web page, we used the example of hosting a GWT application inside a [PHP](http://www.php.net/) page.  Let's now pretend that we have in fact integrated StockWatcher into an existing PHP site.  We already have the following `.php` host page in our project's `src/com/google/gwt/sample/stockwatcher/public` directory:

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">

<html>
  <head>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <title>Wrapper PHP for StockWatcher</title>
    <script type="text/javascript" language="javascript" src="com.google.gwt.sample.stockwatcher.StockWatcher.nocache.js"></script>
  </head>

  <body>
    <?php
      echo 'PHP version: ' . phpversion();
    ?>
    <h1>Stock Watcher</h1>
    <div id="stockList"></div>
  </body>
</html>
```

Now we want to test and debug our PHP+GWT combo in hosted mode.  However, the built-in Tomcat server can't serve `.php` scripts, so we're going to have to use _our_ web server instead.  To make the swap, you'll need to edit the `StockWatcher-shell` hosted mode launch script and change the parameters to `GWTShell`.  For example, if the script ended with the following parameters (this is from a script generated on Windows):

```
$PP_OFF
-out "%~dp0\www" %* com.google.gwt.sample.stockwatcher.StockWatcher/StockWatcher.html
```

you would simply replace those parameters with these:

```
$PP_OFF
-noserver -port 80 -out "%~dp0\www" %* StockWatcher/StockWatcher.php
```

You'll notice that the new `-url` parameter does not specify a domain.  If you exclude the domain,  `GWTShell` will assume that the domain is `localhost`.  Therefore, with these parameters, the hosted mode browser will start at the page:

```
$PP_OFF
http://localhost:80/StockWatcher/StockWatcher.php
```

If you actually run the modified `StockWatcher-shell` script, you'll see that the hosted mode browser now opens to the `StockWatcher.php` page hosted by _our_ web server on `localhost` (notice the PHP version printed at the top):

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherHostedModePHP.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherHostedModePHP.png)

What about debugging, you ask?  Same procedure, except we have to modify our IDE settings instead of the launch script.  In Eclipse, select the _Run -> Open Debug Dialog..._ menu item.  Select the StockWatcher configuration in the tree at left and then the _Arguments_ tab on the right.  Modify the _Program arguments_ in the same way you did the `StockWatcher-shell` script:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherHostedModePHPRunDialog.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherHostedModePHPRunDialog.png)

Now, when you debug the `StockWatcher` project, the hosted mode browser will automatically point to the `.php` version of our host page.  You'll still be able to set breakpoints and step through the code just as before.

For more details on using an alternate server in hosted mode, check out this [FAQ article](FAQ_HostedModeNoServer.md).
