# Communicating with a server #

At some point, most GWT applications will need to interact with a backend server.  GWT provides a couple of different ways to communicate with a server via HTTP.  You can use the [GWT RPC](DevGuideRemoteProcedureCalls.md) framework to transparently make calls to Java servlets and let GWT take care of low-level details like object serialization.  Alternatively, you can use GWT's  [HTTP client classes](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/package-summary.html) to build and send custom HTTP requests.

## Specifics ##

[GWT RPC](DevGuideRemoteProcedureCalls.md)
An easy-to-use RPC mechanism for passing Java objects to and from a server over standard HTTP.

[HTTP Calls](DevGuideHttp.md)
For more fine-grained control, [make custom HTTP requests](DevGuideHttpRequests.md) and optionally process the responses using GWT's [JSON](DevGuideJSON.md) and [XML](DevGuideXML.md) client classes.
