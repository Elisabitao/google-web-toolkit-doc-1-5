# Implementing Services #

Every service ultimately needs to perform some processing to order to respond to client requests. Such [server-side](DevGuideServerSide.md) processing occurs in the _service implementation_, which is based on the well-known [servlet](http://java.sun.com/products/servlet/) architecture.
A service implementation must extend [RemoteServiceServlet](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/server/rpc/RemoteServiceServlet.html) and must implement the associated service interface. Note that the service implementation does _not_ implement the asynchronous version of the service interface.


Every service implementation is ultimately a servlet, but rather than extending [HttpServlet](http://java.sun.com/j2ee/sdk_1.3/techdocs/api/javax/servlet/http/HttpServlet.html), it extends [RemoteServiceServlet](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/server/rpc/RemoteServiceServlet.html) instead. `RemoteServiceServlet` automatically handles serialization of the data being passed between the client and the server and invoking the intended method in your service implementation.


## Testing Services During Development ##

The GWT development shell includes an embedded version of Tomcat which acts as a development-time servlet container for testing.  This allows you to debug both server-side code and client-side code when your run your application in hosted mode using a Java debugger.  To automatically load your service implementation, use the `<servlet>` tag within your [module XML](DevGuideModuleXml.md).

For example, if you are creating a servlet for the interface you created for `com.example.foo.client.MyService` with the class `com.example.foo.server.MyServiceImpl`  which extends `RemoteServletServelet` you would add the following line to your [module XML](DevGuideModuleXml.md):

```
<!-- Example servlet loaded into hosted mode tomcat       -->
<servlet path="/myService" class="com.example.foo.server.MyServiceImpl" />
```

The **path** parameter must match the name you use to specify the URL when you instantiate the `MyService` class in your code by calling [ServiceDefTarget.setServiceEntryPoint()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/ServiceDefTarget.html#setServiceEntryPoint(java.lang.String)).

When testing out both the client and server side code in hosted mode, make sure to add the `gwt-servlet.jar` and server-side `.class` files to the hosted mode shell process classpath to make sure it can pick up your servlet implementations.

> _Tip: The first time you run in hosted mode, a `./tomcat` directory is created in your project directory which houses the `web.xml` configuration file. You sometimes need to modify this file if you want to use third party libraries in your servlet code.
>_

## Common Pitfalls ##

Here are some commonly seen errors trying to get RPC running:

  * When you first run your server-side code, a `ClassNotFoundException` is returned.  This most likely means that the class referenced by the servlet element in your GWT module is not on the classpath.  First time developers often forget to compile their servlet classes which is necessary because the `<module>-compile` script only compiles the client-side code.  Note that writing your code in an IDE that compiles as you work usually prevents this problem. Also, make sure that you've added these class files and the `gwt-servlet.jar` to the hosted mode process classpath.
  * When invoking your RPC call, hosted mode displays an error: "Resource not found: url-path" could mean that you have not added a `<servlet>` tag to the [module XML](DevGuideModuleXml.md) that matches the `url-path` specified in the call to `setServiceEntryPoint()` in your module.
  * When running your RPC call, hosted mode displays an excaption `com.google.gwt.user.client.rpc.ServiceDefTarget$NoServiceEntryPointSpecifiedException: Service implementation URL not specified`.  This error means that you may have omitted the call to `setServiceEntryPoint()` after creating your service asynchronous service instance.

## Deploying Services Into Production ##

In production, you can use any servlet container that is appropriate for your application. You need only to ensure that the client code is configured to invoke the service using the URL to which your servlet is mapped by the `web.xml` configuration. See [ServiceDefTarget](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/ServiceDefTarget.html) for how to set the target URL.  See the [example deployment to Tomcat](DevGuideRPCDeployment.md) in this guide for an example of how to deploy to a servlet container.