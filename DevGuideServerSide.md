# Server-side Code #

Everything that happens within your web server is referred to as _server-side_ processing. When your application running in the user's browser needs to interact with your server (for example, to load or save data), it makes an HTTP request across the network using a [remote procedure call (RPC)](DevGuideRemoteProcedureCalls.md). While processing an RPC, your server is executing server-side code.

GWT provides an RPC mechanism based on Java Servlets to provide access to server side resources.  This mechanism includes generation of efficent client and server side code to [serialize](DevGuideSerializableTypes.md) objects across the network using [deferred binding](DevGuideDeferredBinding.md).

> _Tip: Although GWT translates Java into JavaScript for client-side code, GWT does not meddle with your ability to run Java bytecode on your server whatsoever. Server-side code doesn't need to be translatable, so you're free to use any Java library you find useful._

GWT does not limit you to this one RPC mechanism or server side development environment.  You are free to integrate with other RPC mechanisms, such as JSON using the GWT supplied [RequestBuilder](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html) class,  [JSNI](DevGuideJavaScriptNativeInterface.md) methods or a third party library.
