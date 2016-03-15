#summary Adding a call to a server using GWT-RPC.

# Use Remote Procedure Calls #

All GWT applications run as JavaScript code in the end user's web browser.  Frequently, though, you'll want to create more than just a standalone client-side application.  Your application will need to communicate with the web server, sending requests and receiving updates.  Traditional web applications need to fetch brand new HTML pages each time they talk to the web server.  AJAX applications, on the other hand, offload the user interface logic to the client and make asynchronous _remote procedure calls_ to the server to send and receive _only_ the data itself.  This allows your application's UI to be much more responsive and fluid while reducing the bandwidth requirements and the load on your server.

The GWT RPC framework makes it easy for the client and server components of your web application to exchange Java objects over HTTP.  The server-side code that gets invoked from the client is often referred to as a _service_.  The implementation of a GWT RPC service is based on the well-known [Java servlet](http://java.sun.com/products/servlet/) architecture.  Within the client code, you'll use an automatically-generated proxy class to make calls to the service.  GWT will handle serialization of your method calls' arguments and return value.

It's important to note that GWT RPC services are _not_ the same as web services based on  [SOAP](http://www.w3schools.com/soap/soap_intro.asp) or [REST](http://java.sun.com/developer/technicalArticles/WebServices/restful/).  They were designed simply as a lightweight method for transferring data between your server and the GWT application on the client.  The Developer Guide contains more details on the different [architectural options](DevGuideArchitecturalPerspectives.md) you have for integrating RPC services into your application.

## StockPriceService ##

To help you get some experience with GWT RPC, we're going to add an RPC service to StockWatcher.  We'll call it "StockPriceService."  As you can probably guess from its name, it will provide stock price data to the client-side application.  For simplicity's sake, we'll reuse the existing logic for randomly generating the values, except now that code will be running on the server.  You could just as easily modify the service to instead retrieve actual stock quotes from a database or another remote web service.

## The service interface ##

The first building block of a service is its interface.  In GWT, an RPC service is defined by an interface that extends the [RemoteService](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/RemoteService.html) interface.  Our StockPriceService interface needs only one method, which will accept an array of stock symbols and return the stocks' prices as an array of StockPrice objects.  Go ahead and add a new file named `StockPriceService.java` to the `com.google.gwt.sample.stockwatcher.client` package with this code:

```
package com.google.gwt.sample.stockwatcher.client;

import com.google.gwt.user.client.rpc.RemoteService;
import com.google.gwt.user.client.rpc.RemoteServiceRelativePath;

@RemoteServiceRelativePath("stockPrices")
public interface StockPriceService extends RemoteService {
 
  StockPrice[] getPrices(String[] symbols);
}
```

Pretty straightforward, really.  The only interesting bit is the `@RemoteServiceRelativePath` annotation.  This associates our service with a default path relative to the module base URL.

Since StockPriceService is just an interface, it doesn't actually _do_ anything.  For that, we'll need to write the _service implementation_ class.

## The service implementation ##

The service implementation contains the code that executes when a service is invoked through one of its methods.  In GWT, there's an important distinction between the implementation of an RPC service and the rest of your application.  Since the service implementation lives on the _server_, it is _not_ translated to JavaScript, as your client-side code is.  The service will run as Java bytecode, which means it will have access to the full [Java Platform](http://java.sun.com/javase/) libraries and any other third-party components you may want to integrate.

### The implementation class ###

To implement an RPC service, you'll need to create a class that implements the service interface.  It also needs to extend the [RemoteServiceServlet](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/server/rpc/RemoteServiceServlet.html) class, which handles the necessary RPC plumbing.

Let's create the service implementation for StockPriceService now.  In Eclipse, open the _New Java Class_ dialog (_File -> New -> Class_).

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockPriceServiceImpl.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockPriceServiceImpl.png)

By convention, the service implementation class is named after the service interface with the suffix `Impl`, so we'll call our new class `StockPriceServiceImpl`.  As we mentioned above, it will need to implement the service interface (`StockPriceService`) and extend the [RemoteServiceServlet](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/server/rpc/RemoteServiceServlet.html) class.  It will also need to live in a different package (`com.google.gwt.sample.stockwatcher.server`).  This tells GWT that it is server-side code and does not need to be translated to JavaScript.

Once you click _Finish_ in the dialog it will add the following new class to the StockWatcher project:

```
package com.google.gwt.sample.stockwatcher.server;

import com.google.gwt.sample.stockwatcher.client.StockPrice;
import com.google.gwt.sample.stockwatcher.client.StockPriceService;
import com.google.gwt.user.server.rpc.RemoteServiceServlet;

public class StockPriceServiceImpl extends RemoteServiceServlet implements
    StockPriceService {

  @Override
  public StockPrice[] getPrices(String[] symbols) {
    // TODO Auto-generated method stub
    return null;
  }

}
```

All that's left now is to fill in our one lone service implementation method, getPrices(String[.md](.md)).  For that, we'll borrow the code from our soon-to-be-deprecated refreshWatchList() method in `StockWatcher.java`.

Here's our updated `StockPriceServiceImpl.java`:

```
package com.google.gwt.sample.stockwatcher.server;

import com.google.gwt.sample.stockwatcher.client.StockPrice;
import com.google.gwt.sample.stockwatcher.client.StockPriceService;
import com.google.gwt.user.server.rpc.RemoteServiceServlet;

import java.util.Random;

public class StockPriceServiceImpl extends RemoteServiceServlet implements StockPriceService {

  private static final double MAX_PRICE = 100.0; // $100.00
  private static final double MAX_PRICE_CHANGE = 0.02; // +/- 2%
  
  public StockPrice[] getPrices(String[] symbols) {
    Random rnd = new Random();
    
    StockPrice[] prices = new StockPrice[symbols.length];
    for (int i=0; i<symbols.length; i++) {
      double price = rnd.nextDouble() * MAX_PRICE;
      double change = price * MAX_PRICE_CHANGE * (rnd.nextDouble() * 2f - 1f);
      
      prices[i] = new StockPrice(symbols[i], price, change);
    }
    
    return prices;
  }
}
```

Our service implementation should look familiar, but there is one subtle difference.  Did you notice the new import of `java.util.Random`?  Good for you!  For bonus points, why did we make that change?

If you're stumped, remember that this code is now running on the server.  That means we  use the "real" `java.util.Random` class from the Java runtime library instead of the emulated GWT version in `com.google.gwt.user.client.Random`.  When you're writing your own RPC services in the future, remember: _the service implementation runs on the server as Java bytecode, so you can use any Java class or library you want without worrying about translation to JavaScript._

### Testing the implementation ###

Just as [hosted mode](DevGuideHostedMode.md) makes it easy to test and debug your client-side code, it can do the same for your Java server-side code.  The embedded [Tomcat](http://tomcat.apache.org/) server can host the servlets that contain your service implementation.  All you have to do is add a `<servlet>` element to your [module's XML file](DevGuideModuleXml.md) pointing to the implementation class.  You'll also have to provide a `path` attribute to map the service to a URL.  The URL should be in the form of an absolute directory path (e.g., `/spellcheck` or `/common/login`).  If you specified a default service path with a `@RemoteServiceRelativePath` annotation on the service interface (as we did with StockPriceService), you'll want to make sure the `path` attribute matches the annotation value.

Let's add a `<servlet>` tag to the `StockWatcher.gwt.xml` module file:

```
<module>
  <!-- Inherit the core Web Toolkit stuff.                        -->
  <inherits name='com.google.gwt.user.User'/>

  <!-- Inherit the default GWT style sheet.  You can change       -->
  <!-- the theme of your GWT application by uncommenting          -->
  <!-- any one of the following lines.                            -->
  <inherits name='com.google.gwt.user.theme.standard.Standard'/>
  <!-- <inherits name="com.google.gwt.user.theme.chrome.Chrome"/> -->
  <!-- <inherits name="com.google.gwt.user.theme.dark.Dark"/>     -->

  <!-- Other module inherits                                      -->


  <!-- Specify the app entry point class.                         -->
  <entry-point class='com.google.gwt.sample.stockwatcher.client.StockWatcher'/>
  <servlet path="/stockPrices" class="com.google.gwt.sample.stockwatcher.server.StockPriceServiceImpl" />

  <!-- Specify the application specific style sheet.              -->
  <stylesheet src='StockWatcher.css' />	
</module>
```

Since we're mapping the StockPriceService to `/stockPrices`, the full URL will be:

```
$PP_OFF
http://localhost:8888/com.google.gwt.sample.stockwatcher.StockWatcher/stockPrices
```

Ok, the server half of our new StockPriceService should be done.  Before we start working on the client-side code, though, we need to have a talk about _asynchronous calls_.

## Asynchronous calls ##

All RPC calls you make in GWT are asynchronous, which means they don't block while waiting for the call to return.  There are several benefits to using that approach instead of simpler, synchronous (blocking) calls:

  * Your user interface remains responsive.  The JavaScript engines in web browsers are generally single-threaded, so calling a server synchronously causes the web page to "hang" until the call completes.  If the network is slow or the server is unresponsive, this could ruin the end-user experience.  Synchronous server calls also go against the very principles of AJAX (_**Asynchronous** JavaScript And XML_).

  * You can make multiple server calls at the same time.  _Note, however, that browsers typically restrict the number of outgoing network connections to two at a time, limiting just how much parallelism you can add using async calls_.

  * You can perform other work while waiting on a pending server call.  For example, you can build up your user interface while simultaneously retrieving the data from the server to populate the interface.  This shortens the overall time it takes for the user to see the data on the page.

Asynchronous calls do take some getting used to, though.  Since an async call invocation does not block, the code following the call executes immediately.  When the call completes, it will run code within a callback method you specified when you made the call.

The way you specify your callback methods is by passing an [AsyncCallback](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html) object to the service proxy class when you invoke one of the service's methods.  The callback object must contain two methods: [onFailure(Throwable)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html#onFailure(java.lang.Throwable)) and [onSuccess(T)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html#onSuccess(T)).  When the server call completes, one of these two methods will be called depending on whether the call succeeded or failed.

To add an [AsyncCallback](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html) parameter to all of our service methods we're going to have to create a new interface with new method definitions.  This asynchronous version will be very similar to the original service interface, with a few important differences:

  * It must have the same name as the service interface, with an `Async` added to the end.
  * It must be located in the same package as the service interface
  * Each method must have the same name and signature as the service interface, except with no return type and an [AsyncCallback](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html) object as the last parameter.

Let's apply these rules to create an asynchronous interface for StockPriceService.  Add a new file named `StockPriceServiceAsync.java` to the project with the following code:

```
package com.google.gwt.sample.stockwatcher.client;

import com.google.gwt.user.client.rpc.AsyncCallback;

public interface StockPriceServiceAsync {

  void getPrices(String[] symbols, AsyncCallback<StockPrice[]> callback);
}
```

You might be curious about the lack of a return value from this new method.  If you think about, you'll realize we really don't have a choice: since an async call returns immediately, we can't possibly get the return value from the call invocation.  So just where _does_ it come from?  The answer is the `callback` parameter we just added.  If the call completes successfully, the return value will show up as a parameter to our [onSuccess(T)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html#onSuccess(T)) callback method.  You'll see that in action in just a bit.

## Client side ##

All the pieces are now in place to call the RPC service from the client.  Here are the steps we'll need to take (don't worry about typing them in just yet; just read them through to get familiar with the process).

#### 1. Create the service proxy class ####

Call [GWT.create(Class)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class)) to create an instance of the service proxy class:

```
StockPriceServiceAsync stockPriceSvc = GWT.create(StockPriceService.class);
```

#### 2. Set up the callback ####

Create a new instance of an [AsyncCallback](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html) object.

When the RPC call completes, if all goes well, our [onSuccess(T)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html#onSuccess(T)) will be called and we'll receive the return value as the `result` argument.

If something goes wrong with the call, GWT will call our  [onFailure(Throwable)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html#onFailure(java.lang.Throwable)) method, passing us the exception:

```
AsyncCallback<StockPrice[]> callback = new AsyncCallback<StockPrice[]>() {
  public void onFailure(Throwable caught) {
    // do something with errors
  }

  public void onSuccess(StockPrice[] result) {
    // update the watch list FlexTable
  }
};
```

#### 3. Make the call ####

All that's left now is to actually make the call:

```
stockPriceSvc.getPrices(symbols, callback);
```

### Changes to StockWatcher ###

Now that you know what needs to be done, let's add an RPC call to StockWatcher.  Open up the `StockWatcher.java` file.

The first order of business is to add a new `import` statements to pull in the necessary  types:

```
import com.google.gwt.core.client.GWT;
import com.google.gwt.user.client.rpc.AsyncCallback;
```

Next, create a new instance field to hold the service proxy reference:

```
private StockPriceServiceAsync stockPriceSvc;
```

Finally, replace the old refreshWatchList() function with this revamped version:

```
private void refreshWatchList() {
  // lazy initialization of service proxy
  if (stockPriceSvc == null) {
    stockPriceSvc = GWT.create(StockPriceService.class);
  }
  
  AsyncCallback<StockPrice[]> callback = new AsyncCallback<StockPrice[]>() {
    public void onFailure(Throwable caught) {
      // do something with errors
    }

    public void onSuccess(StockPrice[] result) {
      updateTable(result);
    }
  };
  
  // make the call to the stock price service
  stockPriceSvc.getPrices(stocks.toArray(new String[0]), callback);
}
```

You should be able to follow the new code without too much difficulty.  Notice that we can cache the service proxy for future calls to the service.  When our call to the remote getPrices(String[.md](.md)) method returns, it will either update our watch list [FlexTable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/FlexTable.html) or do nothing if the call fails (don't worry; we'll add better error handling later).

Ready to see GWT RPC in action?  Ok, go ahead and launch the new StockWatcher and try adding a new stock to the list.  Since we've changed the module XML file, you'll need to restart hosted mode, not just refresh it.  The application should look the same as before, except...

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherSerializationError.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherSerializationError.png)

Er, oops.  _That_ wasn't supposed to happen.  So what went wrong?  Looking in the hosted mode Development Shell log, we see the problem: our StockPrice class is not _serializable_.

## Serialization ##

Anytime you transfer an object over the network via GWT RPC, it needs to be serialized.  Serialization is the process of packaging the contents of an object so that it can moved from one application to another application or stored for later use.  GWT RPC requires that all service method parameters and return types be serializable.

Note that although the two are similar, GWT serialization is not the same as serialization based on the Java [Serializable interface](http://java.sun.com/j2se/1.5.0/docs/api/java/io/Serializable.html).

### Serialization requirements ###

So what makes a type serializable?  To start with, all primitive types (`int`, `char`, `boolean`, etc.) and their wrapper objects are serializable by default.  Arrays of serializable types are also by extension serializable.  Classes are serializable if they meet a few basic requirements:

  1. It implements [IsSerializable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/IsSerializable.html) or [Serializable](http://java.sun.com/j2se/1.5.0/docs/api/java/io/Serializable.html), either directly or because it derives from a superclass that does.
  1. Its non-final, non-transient instance fields are themselves serializable, and
  1. It has a default (zero argument) constructor with any access modifier (e.g. private Foo(){} will work)

GWT honors the `transient` keyword, so values in those fields are not serialized (and thus, not sent over the wire when used in RPC calls).

If you're curious, the Developer Guide has more details about [serialization in RPC](DevGuideSerializableTypes.md).

### Fixing StockPrice ###

Armed with our newfound knowledge of GWT serialization, let's fix the StockPrice class to make it RPC-ready.  Since all our instance fields are primitive types, all we really need to do is implement [IsSerializable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/IsSerializable.html):

```
package com.google.gwt.sample.stockwatcher.client;

import com.google.gwt.user.client.rpc.IsSerializable;

public class StockPrice implements IsSerializable {
  
  private String symbol;
  private double price;
  private double change;
  
  ...
```

Launch StockWatcher again.  Success!  It won't look any different on the surface, but underneath, StockWatcher is now retrieving its stock price updates from our server-side StockPriceService servlet instead of generating them on the client-side.  Since we have basic RPC working, let's move right along and add some proper error handling.

## Handling exceptions ##

In our current refreshWatchList() implementation, our [AsyncCallback](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html) object's [onFailure(Throwable)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html#onFailure(java.lang.Throwable)) is pretty useless.  It's empty, so any exceptions that occur during the RPC call will be ignored.  That means if the user's network connection goes down: no more price updates!  Unless he notices the stale last updated timestamp at the bottom, he might not realize the numbers he's seeing are not current (_Gee, the market's really slow today... is today a holiday or something?_).  We'd hate for one of our users to make an ill-timed trade based on incorrect prices in StockWatcher, so let's give them an indication when something goes wrong.

A GWT RPC call can fail for one of two reasons: an _unexpected_ exception or a _checked_ exception.

### Unexpected exceptions ###

There are lots of things that could break an RPC server call.  The network could be down, the HTTP server on the other end might not be listening, our DNS server could be on fire, and so on.  When GWT has a problem invoking the service, it will pass a [InvocationException](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/InvocationException.html) to your [onFailure(Throwable)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html#onFailure(java.lang.Throwable)) method describing the problem.

Another type of unexpected exception can occur if GWT is able to invoke the service method, but the service implementation threw an undeclared exception.  For example, a bug may cause a [NullPointerException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/NullPointerException.html).  When unexpected exceptions occur in the service implementation, you can find the full stack trace in the hosted mode log:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockPriceServiceImplNullPtr.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockPriceServiceImplNullPtr.png)

On the client-side, your [onFailure(Throwable)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html#onFailure(java.lang.Throwable)) callback method will receive an [InvocationException](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/InvocationException.html) with a rather generic message: `The call failed on the server; see server log for details`.

### Checked exceptions ###

If we know a service method may throw a particular type of exception and we want the client-side code to be able to handle it, we can use _checked exceptions_.  GWT supports the `throws` keyword so you can add it to your service interface methods as needed.  When checked exceptions occur in an RPC service method, GWT will serialize the exception and send it back to the caller on the client for handling.

### Throwing an exception from StockPriceServiceImpl ###

Ok, so let's see an example of this.  For illustration purposes, suppose we want to throw an exception when the user tries to retrieve stock price data for a stock that has been delisted.  The first step is to create the custom exception class, which we'll call `DelistedException`:

```
package com.google.gwt.sample.stockwatcher.client;

import com.google.gwt.user.client.rpc.IsSerializable;

public class DelistedException extends Exception implements IsSerializable {
  
  private String symbol;
  
  public DelistedException() {
  }
  
  public DelistedException(String symbol) {
    this.symbol = symbol;
  }
  
  public String getSymbol() {
    return this.symbol;
  }  
}
```

Notice that since this exception may have to be sent over RPC we have to mark the class as serializable with [IsSerializable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/IsSerializable.html).

Next, we need to add the `throws` declaration to our service method interface in `StockPriceService.java`, along with an accompanying `import` statement:

```
import com.google.gwt.sample.stockwatcher.client.DelistedException;
```

```
StockPrice[] getPrices(String[] symbols) throws DelistedException;
```

We also need to modify the actual service implementation in `StockPriceServiceImpl.java`.  The first change is the `throws` on the getPrices(String[.md](.md)) method.  The other addition is code to actually throw the `DelistedException`.  We'll just keep it simple and throw the exception whenever the stock symbol `ERR` added to the user's watch list.

```
import com.google.gwt.sample.stockwatcher.client.DelistedException;
```

```
public StockPrice[] getPrices(String[] symbols) throws DelistedException {
  Random rnd = new Random();
  
  StockPrice[] prices = new StockPrice[symbols.length];
  for (int i=0; i<symbols.length; i++) {
    if (symbols[i].equals("ERR")) {
      throw new DelistedException("ERR");
    }

    double price = rnd.nextDouble() * MAX_PRICE;
    double change = price * MAX_PRICE_CHANGE * (rnd.nextDouble() * 2f - 1f);
    
    prices[i] = new StockPrice(symbols[i], price, change);
  }
  
  return prices;
}
```

That's it.  We don't need to add the `throws` declaration to the service method in `StockPriceServiceAsync.java`.  That method will always return immediately (remember, it's asynchronous), so we'll instead receive any thrown checked exceptions when GWT calls our [onFailure(Throwable)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html#onFailure(java.lang.Throwable)) callback method.

Now, to the client-side code.  We need to decide exactly what to do when we catch an exception from an RPC call.  We could display a message box using [Window.alert(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/Window.html#alert(java.lang.String)), but that'd be taking the easy way out.  Besides, if we encounter multiple errors the user could end up with a whole stack of message boxes if the errors occur while he's away from his desktop.  That's not nice.  Let's go ahead and add a new [Label](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Label.html) widget to display errors without annoying the user.

We'll want the errors to stand out, so let's start by adding a new CSS rule to `StockWatcher.css`:

```
.errorMessage {
  color: Red;
}
```

Next, add the new widget to display the error message.  In `StockWatcher.java`, add the following new instance field:

```
private Label errorMsgLabel = new Label();
```

Initialize it in onModuleLoad():

```
...
// assemble Add Stock panel
addPanel.add(newSymbolTextBox);
addPanel.add(addButton);
addPanel.addStyleName("addPanel");

// assemble main panel
errorMsgLabel.setStyleName("errorMessage");
errorMsgLabel.setVisible(false);

mainPanel.add(errorMsgLabel);
mainPanel.add(stocksFlexTable);
mainPanel.add(addPanel);
mainPanel.add(lastUpdatedLabel);
```

And now, the final step: handling the error in refreshWatchList().  Fill in the onFailure(Throwable) callback method as follows:

```
public void onFailure(Throwable caught) {
  // display the error message above the watch list
  String details = caught.getMessage();
  if (caught instanceof DelistedException) {
    details = "Company '" + ((DelistedException)caught).getSymbol() + "' was delisted";
  }
    
  errorMsgLabel.setText("Error: " + details);
  errorMsgLabel.setVisible(true);
}
```

Almost forgot: the final, _final_ step is to hide the error message widget at the end of updateTable() so if the error is corrected (server comes back online, `ERR` symbol removed from the table, etc.), the error disappears:

```
private void updateTable(StockPrice[] prices) {
  for (int i=0; i<prices.length; i++) {
    updateTable(prices[i]);
  }
  
  // change the last update timestamp
  lastUpdatedLabel.setText("Last update : " + 
      DateTimeFormat.getMediumDateTimeFormat().format(new Date()));

  // clear any errors
  errorMsgLabel.setVisible(false);
}
```

Time to test our changes.  Launch StockWatcher again (_the changes will not take effect if we just refresh the hosted mode browser, since we modified the service_) and add the `ERR` symbol to the watch list:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDelistedException.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDelistedException.png)

If you switch to the hosted mode Development Shell window, you won't see any exception messages in the log, since the exception was checked (and thus, expected).

## Deploying services into production ##

During testing, you can use [hosted mode's](DevGuideHostedMode.md) built-in [Tomcat](http://tomcat.apache.org/) server to test the server-side code for your RPC services.  When you deploy your GWT application, you can use any servlet container to host your services.  Just make sure your client code invokes the service using the URL the servlet is mapped to in the `web.xml` configuration file.  For more details about deploying RPC servlets, see [this article](DevGuideRPCDeployment.md).

## More details ##

To learn more, read up on the [Remote Procedure Calls](DevGuideRemoteProcedureCalls.md) section in the Developer Guide.