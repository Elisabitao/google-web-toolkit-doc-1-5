# Making HTTP requests #

GWT contains a set of [HTTP client classes](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/package-summary.html) that allow your application to make generic HTTP requests.

## Using HTTP in GWT ##

Making HTTP requests in GWT works much like it does in any language or framework, but there are a couple of important differences you should be aware of.

First, because of the single-threaded execution model of most web browsers, long synchronous operations such as server calls can cause a JavaScript application's interface (and sometimes the browser itself) to become unresponsive.  To prevent network or server communication problems from making the browser "hang", GWT allows only _asynchronous_ server calls.  When sending an HTTP request, the client code must register a callback method that will handle the response (or the error, if the call fails).  For more information about the exclusion of synchronous server connections, you may want to check out [this FAQ article](FAQ_SynchronousServerConnection.md).

Second, because GWT applications run as JavaScript within a web page, they are subject to the browser's [same origin policy (SOP)](http://en.wikipedia.org/wiki/Same_origin_policy).  SOP prevents client-side JavaScript code from interacting with untrusted (and potentially harmful) resources loaded from other websites.  In particular, SOP makes it difficult (although not impossible) to send HTTP requests to servers other than the one that hosts your GWT application.  [This FAQ article](FAQ_SOP.md) goes into more detail about SOP and its implication for GWT developers.  To see an example of SOP in action (and to learn how to work around it), check out the [JSON/HTTP](GettingStartedJSON.md) chapter in the [Getting Started guide](GettingStarted.md).

## HTTP client types ##

To use the [HTTP types](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/package-summary.html) in your application, you'll need to first inherit the GWT HTTP module by adding the following `<inherits>` tag to your [module XML file](http://code.google.com/p/google-web-toolkit-doc-1-5/wiki/DevGuideModuleXml):

```
<inherits name="com.google.gwt.http.HTTP" />
```

[RequestBuilder](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html) is the core class you'll need for constructing and sending HTTP requests.  Its [constructor](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html#RequestBuilder(com.google.gwt.http.client.RequestBuilder.Method,%20java.lang.String)) has parameters for specifying the HTTP method of the request (GET, POST, etc.) and the URL (the [URL](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/URL.html) utility class is handy for escaping invalid characters).  Once you have a RequestBuilder object, you can use its methods to set the [username](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html#setUser(java.lang.String)), [password](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html#setPassword(java.lang.String)), and [timeout interval](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html#setTimeoutMillis(int)).  You can also set any number of  [headers](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html#setHeader(java.lang.String,%20java.lang.String)) in the HTTP request.

Once the HTTP request is ready, call the server using the [sendRequest(String, RequestCallback)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html#sendRequest(java.lang.String,%20com.google.gwt.http.client.RequestCallback)) method.  The [RequestCallback](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestCallback.html) argument you pass will handle the response or the error that results.  When a request completely normally, your [onResponseReceived(Request, Response)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestCallback.html#onResponseReceived(com.google.gwt.http.client.Request,%20com.google.gwt.http.client.Response)) method is invoked.  Details of the response (for example, [status code](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/Response.html#getStatusCode()), [HTTP headers](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/Response.html#getHeaders()), and [response text](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/Response.html#getText())) can be retrieved from the [Response](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/Response.html) argument.  Note that the onResponseReceived(Request, Response) method is called even if the HTTP status code is something other than 200 (success).  If the call _does not_ complete normally, the [onError(Request, Throwable)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestCallback.html#onError(com.google.gwt.http.client.Request,%20java.lang.Throwable)) method gets called, with the second parameter describing the type error that occurred.

As noted before, all HTTP calls in GWT are asynchronous, so the code following the call to [sendRequest(String, RequestCallback)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html#sendRequest(java.lang.String,%20com.google.gwt.http.client.RequestCallback)) will be executed immediately, _not_ after the server responds to the HTTP request.  You can use the [Request](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/Request.html) object that is returned from sendRequest(String, RequestCallback) to [monitor the status](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/Request.html#isPending()) of the call, and [cancel it](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/Request.html#cancel()) if necessary.

Here's a brief example of making an HTTP request to a server:

```
import com.google.gwt.http.client.*;
...

String url = "http://www.myserver.com/getData?type=3";
RequestBuilder builder = new RequestBuilder(RequestBuilder.GET, URL.encode(url));

try {
  Request request = builder.sendRequest(null, new RequestCallback() {
    public void onError(Request request, Throwable exception) {
       // Couldn't connect to server (could be timeout, SOP violation, etc.)     
    }

    public void onResponseReceived(Request request, Response response) {
      if (200 == response.getStatusCode()) {
          // Process the response in response.getText()
      } else {
        // Handle the error.  Can get the status text from response.getStatusText()
      }
    }       
  });
} catch (RequestException e) {
  // Couldn't connect to server        
}
```

For a complete example of using the HTTP client classes, see the [JSON/HTTP](GettingStartedJSON.md) section in the [Getting Started](GettingStarted.md) guide.

## Processing the response ##

Once you receive the response from the server using [Response.getText()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/Response.html#getText()), it's up to you to process it.  If your response is encoded in JSON or XML, you can take advantage of GWT's built-in types to parse the data (see the topics [Working with JSON](DevGuideJSON.md) and [Working with XML](DevGuideXML.md), respectively).
