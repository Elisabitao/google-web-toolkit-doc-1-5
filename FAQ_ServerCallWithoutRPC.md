# How do I make a call to the server if I am not using the GWT RPC? #

The heart of AJAX is making data read/write calls to a server from the JavaScript application running in the browser. GWT is "RPC agnostic" and has no particular requirements about what protocol is used to issue RPC requests, or even what language the server code is written in. GWT does provide a library of classes that makes the RPC communications with a J2EE server extremely easy, but they are not required.

To communicate with your server from the browser without using GWT's RPC, you will need to use an asynchronous server connection.  This involves a few simple steps:

  1. Create a connection to the server, using the browser's XMLHTTPRequest feature.
  1. Construct your payload according to whatever protocol you wish to use, convert it to a string, and send it to the server over the connection.
  1. Receive the server's response payload, and parse it according to the protocol.



GWT provides a library to largely (though not completely) automate these three steps.  Developers should use the [Request Builder](http://google-web-toolkit.googlecode.com/svn/javadoc/1.4/com/google/gwt/http/client/RequestBuilder.html) class and other classes in the `com.google.gwt.http.client.html` package, which contains a clean asynchronous server request callback implementation.  Full documentation for this package can be found at [the API Reference](http://google-web-toolkit.googlecode.com/svn/javadoc/1.4/com/google/gwt/http/client/package-summary.html) for that package.

If you are attempting to use this functionality and are having problems, first check to make sure that you have included the HTTP via the following tag in your .gwt.xml file:

```
    <inherits name="com.google.gwt.http.HTTP"/>
```

> _Note:  New users frequently notice the `com.google.gwt.user.client.HTTPRequest` class, and attempt to use it. However, that particular class is intended primarily for use by GWT's internal RPC implementation, and is not a very friendly interface for developers to use on their own.  In fact, the HTTPRequest class may go away in a future release of GWT.
>_
