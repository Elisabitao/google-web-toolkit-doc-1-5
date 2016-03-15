# Creating Services #

In order to define your RPC interface, you need to:

  1. Define an interface for your service that extends [RemoteService](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/RemoteService.html) and lists all your RPC methods.
  1. Define a class to implement the server-side code that extends [RemoteServiceServlet](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/server/rpc/RemoteServiceServlet.html) and implements the interface you created above.
  1. Define an asynchronous interface to your service to be called from the client-side code.

## Synchronous Interface ##

To begin developing a new service interface, create a [client-side](DevGuideClientSide.md) Java interface that extends the [RemoteService](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/RemoteService.html) tag interface.

```
package com.example.foo.client;

import com.google.gwt.user.client.rpc.RemoteService;

public interface MyService extends RemoteService {
  public String myMethod(String s);
}
```

This synchronous interface is the definitive version of your service's specification. Any implementation of this service on the [server-side](DevGuideServerSide.md) must extend [RemoteServiceServlet](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/server/rpc/RemoteServiceServlet.html) and implement this service interface.

```
package com.example.foo.server;

import com.google.gwt.user.server.rpc.RemoteServiceServlet;
import com.example.client.MyService;


public class MyServiceImpl extends RemoteServiceServlet implements
    MyService {

  public String myMethod(String s) {
    // Do something interesting with 's' here on the server.
    return s;
  }
}
```

> _Tip: It is not possible to call this version of the RPC directly from the client. You must create an asynchronous interface to all your services as shown below.
>_

## Asynchronous Interfaces ##

Before you can actually attempt to make a remote call from the client, you must create another client interface, an asynchronous one, based on your original service interface. Continuing with the example above, create a new interface in the client subpackage:

```
package com.example.foo.client;

interface MyServiceAsync {
  public void myMethod(String s, AsyncCallback<String> callback);
}
```

The nature of asynchronous method calls requires the caller to pass in a callback object that can be notified when an asynchronous call completes, since by definition the caller cannot be blocked until the call completes. For the same reason, asynchronous methods do not have return types; they generally return void.  Should you wish to have more control over the state of a pending request, return [Request](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/Request.html) instead. After an asynchronous call is made, all communication back to the caller is via the passed-in callback object.

## Naming Standards ##

Note the use of the suffix `Async` and argument referencing the `AsyncCallback` class in the examples above.  The relationship between a service interface and its asynchronous counterpart must follow certain naming standards.  The GWT compiler depends on these naming standards in order to generate the proper code to implement RPC.

  * A service interface must have a corresponding asynchronous interface with the same package and name with the Async suffix appended.  For example, if a service interface is named `com.example.cal.client.SpellingService`, then the asynchronous interface must be called `com.example.cal.client.SpellingServiceAsync`.
  * Each method in the synchronous service interface must have a coresponding method in the asynchronous service interface with an extra [AsyncCallback](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html) parameter as the last argument.

See [AsyncCallback](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html) for additional details on how to implement an asynchronous callback.