# HTTP Calls #

If your GWT application needs to communicate with a server, but you can't use [Java servlets](http://java.sun.com/products/servlet/) on the backend, you'll need to use direct HTTP instead of GWT's [RPC framework](DevGuideRemoteProcedureCalls.md).  GWT contains a number of HTTP client classes that simplify making custom HTTP requests to your server and optionally processing a [JSON](http://www.json.org/)-  or [XML](http://www.w3.org/XML/)-formatted response.

## Specifics ##

[Making HTTP requests](DevGuideHttpRequests.md) Use the [RequestBuilder](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html) and other classes in the [com.google.gwt.http.client](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/package-summary.html) package to build and send HTTP requests.

[Working with JSON](DevGuideJSON.md) Parse and create JSON encoded data using the [JSON client types](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/package-summary.html).

[Working with XML](DevGuideXML.md) Parse and create XML documents using GWT's [XML processing types](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/xml/client/package-summary.html).
