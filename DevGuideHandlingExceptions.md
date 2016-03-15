# Handling Exceptions #

Making RPCs opens up the possibility of a variety of errors. Networks fail, servers crash, and problems occur while processing a server call. GWT lets you handle these conditions in terms of Java exceptions. RPC-related exceptions fall into two categories: checked exceptions and unchecked exceptions.

Keep in mind that any custom exceptions that you want to define, like any other piece of client-side code, must only be composed of types supported by [GWT's emulated JRE library](RefJreEmulation.md).

## Checked Exceptions ##

[Service interface](DevGuideCreatingServices.md) methods support `throws` declarations to indicate which exceptions may be thrown back to the client from a service implementation. Callers should implement [AsyncCallback.onFailure(Throwable)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html#onFailure(java.lang.Throwable)) to check for any exceptions specified in the service interface.

## Unexpected Exceptions ##

### InvocationException ###

An RPC may not reach the [service implementation](DevGuideImplementingServices.md) at all. This can happen for many reasons: the network may be disconnected, a DNS server might not be available, the HTTP server might not be listening, and so on. In this case, an [InvocationException](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/InvocationException.html) is passed to your implementation of [AsyncCallback.onFailure(Throwable)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html#onFailure(java.lang.Throwable)). The class is called ` InvocationException` because the problem was with the invocation attempt itself rather than with the service implementation.

An RPC can also fail with an invocation exception if the call does reach the server, but an undeclared exception occurs during normal processing of the call. There are many reasons such a situation could arise: a necessary server resource, such as a database, might be unavailable, a `NullPointerException` could be thrown due to a bug in the service implementation, and so on. In these cases, a [InvocationException](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/InvocationException.html) is thrown in application code.

### IncompatibleRemoteServiceException ###

Another type of failure can be caused by an incompatibility between the client and the server. This most commonly occurs when a change to a [service implementation](DevGuideImplementingServices.md) is deployed to a server but out-of-date clients are still active. For more details please see [IncompatibleRemoteServiceException](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/IncompatibleRemoteServiceException.html).

When the client code receives an [IncompatibleRemoteServiceException](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/IncompatibleRemoteServiceException.html), it should ultimately attempt to refresh the browser in order to pick up the latest client.
