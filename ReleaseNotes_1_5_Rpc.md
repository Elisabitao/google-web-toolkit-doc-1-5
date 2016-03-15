# Enhancements to the GWT RPC components #

### Java 1.5 syntax supported ###
GWT 1.5 supports and honors the Java 1.5 constructs like generics and annotations. This means, for example, that rather than using the @gwt.typeArgs JavaDoc annotation for types transferred through RPC, you can (and should) use real type parameters instead.

### Asynchronous interface improvements - access underlying HTTP request objects and RPC payloads ###
There have been a couple of major improvements to the asynchronous interface component in GWT RPC.

The first is that asynchronous interface methods can now return the underlying HTTP request object ([http.client.Request](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/Request.html)) so you can access and tweak it as necessary for your application needs before sending it off through RPC. Asynchronous interface methods can now also return void or [http.client.RequestBuilder](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html) objects.

The second improvement is that the asynchronous interface proxy can now be cast to a [SerializationStreamFactory](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/SerializationStreamFactory.html) so you can now process the RPC payload independently from the RPC transport system.

### Use annotations to set the service entry point ###
The [RemoteServiceRelativePath](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/RemoteServiceRelativePath.html) annotation can now be used to specify the default service entry point path; no need to call setServiceEntryPoint(String).

### Failed RPC calls can now be detected ###
RPC calls that do not receive a 200 OK response can now be detected by casting
the onFailure() throwable to [StatusCodeException](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/StatusCodeException.html).

### Serializable objects may now have non-public, no-arg constructors ###

Previous to 1.5, GWT required that any Serializable objects expected to be transferred through RPC must define a public no-arg constructor. These types can now define those constructors as private or protected and still be transferred through RPC.

### SerializableException deprecated ###

The [SerializableException](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/SerializableException.html) class is deprecated since java.lang.Throwable now implements Serializable.
