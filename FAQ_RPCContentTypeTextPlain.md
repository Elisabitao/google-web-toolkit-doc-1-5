# ServletException: Content-Type must be 'text/plain' #
If you are using GWT RPC and upgrading from GWT 1.4 to GWT 1.5, you may see a stack trace similar to the following:

```
$PP_OFF
javax.servlet.ServletException: Content-Type must be 'text/plain' with 'charset=utf-8' (or unspecified charset)
        at com.google.gwt.user.server.rpc.RemoteServiceServlet.readPayloadAsUtf8(RemoteServiceServlet.java: 119)
        at com.google.gwt.user.server.rpc.RemoteServiceServlet.doPost(RemoteServiceServlet.java: 178)
        at javax.servlet.http.HttpServlet.service(HttpServlet.java: 738)
        at javax.servlet.http.HttpServlet.service(HttpServlet.java: 831)
        ...
```

In GWT 1.5, the RPC content type was switched from "text/plain" to "text/x-gwt-rpc".

If you receive this exception, then your server is still using a GWT 1.4 gwt-servlet.jar.

### Workaround ###
Update your server to use the GWT 1.5 gwt-servlet.jar.
