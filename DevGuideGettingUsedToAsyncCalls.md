# Getting Used to Asynchronous Calls #

Using the [AsyncCallback](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html) interface is new to many developers.  Code is not necessarily executed sequentially, and it forces developers to handle situations where a server call is in progress but has not completed.  Despite these seeming drawbacks, the designers of GWT felt it was an essential part of creating usable interfaces in AJAX for several reasons:

  * The JavaScript engine in most browsers is singly threaded, meaning that:
    * The JavaScript engine will hang while waiting for a synchronous RPC call to return.
    * Most browsers will popup a dialog if any JavaScript entry point takes longer than a few moments.
  * A server could be inaccessible or otherwise non responsive.  Without an asynchronous mechanism, the application could be left waiting on a server call to return indefinitely.
  * Asynchronous mechanisms allow server calls to run in parallel with logic inside the application, shortening the overall response time to the user.
  * Asynchronous mechanisms can service multiple server calls in parallel.

Browser mechanics aside, asynchronous RPC gives your application the ability to achieve true parallelism in your application, even without multi-threading.
For example, suppose your application displays a large [table](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLTable.html) containing many widgets. Constructing and laying out all those widgets can be time consuming. At the same time, you need to fetch data from the server to display inside the table. This is a perfect reason to use asynchronous calls. Initiate an asynchronous call to request the data immediately before you begin constructing your table and its widgets. While the server is fetching the required data, the browser is executing your user interface code. When the client finally receives the data from the server, the table has been constructed and laid out, and the data is ready to be displayed.

To give you an idea of how effective this technique can be, suppose that building the table takes one second and fetching the data takes one second. If you make the server call synchronously, the whole process will require at least two seconds.

> ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/GettingUsedToAsyncCalls1.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/GettingUsedToAsyncCalls1.png)

But if you fetch the data asynchronously, the whole process still takes just one second, even though you are doing two seconds' worth of work.

> ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/GettingUsedToAsyncCalls2.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/GettingUsedToAsyncCalls2.png)


> _Tip: Most browsers restrict the number of outgoing network connections to two at a time, restricting the amount of parallelism you can expect from multiple simultaneous RPCs.
>_

The hardest thing to get used to about asynchronous calls is that the calls are non-blocking, however, Java inner classes go a long way toward making this manageable.  Consider the following implementation of an asynchronous call adapted from the [Dynamic Table](http://code.google.com/webtoolkit/examples/dynamictable/) sample application.  It uses a slightly different syntax to define the required interface for the [AsyncCallback](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html) object that is the last parameter to the `getPeople` RPC call:

```

 // This code is called before the RPC starts
 //
  if (startRow == lastStartRow) {
	...
  }

  // Invoke the RPC call, implementing the callback methods inline:
  //
  calService.getPeople(startRow, maxRows, new AsyncCallback<Person[]>() {

    // When the RPC returns, this code will be called if the RPC fails
    public void onFailure(Throwable caught) {
       statusLabel.setText("Query failed: " + caught.getMessage());
       acceptor.failed(caught);
    }

    // When the RPC returns, this code is called if the RPC succeeds
    public void onSuccess(Person[] result) {
      lastStartRow = startRow;
      lastMaxRows = maxRows;
      lastPeople = result;
      pushResults(acceptor, startRow, result);
      statusLabel.setText("Query reutrned " + result.length + " rows.");
    }
  });

  // The above method call will not block, but return immediately.
  // The following code will execute while the RPC is in progress, 
  // before either of onFailure() or onSuccess() are executed.
  //
  statusLabel.setText("Query in progress...");
  ...

```

The important issue to understand is that that the code that follows the RPC call invocation will be executed while the actual round trip to the server is still in progress.  Although the code inside the `onSuccess()` method is defined inline with the call,  it will not be executed until both the calling code returns back to the JavaScript main loop, and the result message from the server returns.
