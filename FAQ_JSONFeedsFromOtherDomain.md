# How can I dynamically fetch JSON feeds from other web domains? #

Like all AJAX tools, GWT's HTTP client and RPC libraries are restricted to only accessing data from the same site where your application was loaded, due to the [browser Same Origin Policy](FAQ_SOP.md). If you are using JSON, you can work around this limitation using a 

&lt;SCRIPT&gt;

 tag. To accomplish this, though, you must resort to using DOM manipulation methods to inject the  tag into your host page dynamically.

First, you need an external JSON service which can invoke user defined callback functions with the JSON data as argument. An example of such a service is [GData's "alt=json-in-script&callback=myCallback" support](http://code.google.com/apis/gdata/json.html). Then, you can generate a [bridge method](DevGuideJavaFromJavaScript.md), which will be used to invoke a GWT JSON callback handler.

Here is an example:

```
package mypackage;
public class MyJSONUtility
{
   interface JSONHandler
   {
      public void handleJSON(JavaScriptObject obj);
   }

   public static native void makeJSONRequest(String url, JSONHandler handler) /*-{
       $wnd.jsonCallback = function(jsonObj) {
         @mypackage.MyJSONUtility::dispatchJSON(Lcom/google/gwt/core/client/JavaScriptObject;Lcom/google/gwt/sample/client/JsonTestApp$JSONHandler;)(jsonObj, handler);
       }
      
       // create SCRIPT tag, and set SRC attribute equal to JSON feed URL + callback function name
       var script = $wnd.document.createElement("script");
       script.setAttribute("src", url+"jsonCallback");
       script.setAttribute("type", "text/javascript");
       
       $wnd.document.getElementsByTagName("head")[0].appendChild(script);
   
   }-*/;

   public static void dispatchJSON(JavaScriptObject jsonObj, JSONHandler handler)
   {
      handler.handleJSON(jsonObj);
   }
}
```

You can then implement the JSONHandler interface as you choose, and call

```
   MyJSONUtility.makeJSONRequest(
       "http://www.google.com/calendar/feeds/developer-calendar@google.com/public/full?alt=json-in-script&callback=",
       myHandlerInstance);
```

Of course, the above example doesn't handle multiple concurrent requests. For that, you need an array of bridge methods; you would access them during callbacks using syntax similar to
```
jsonCallbacks[requestNumber].
```

If the external feed doesn't support callback function names using array syntax, you can generate uniquely named bridge methods like jsonCallback1, jsonCallback2, etc. Also, don't forget to cleanup in the bridge function after it's been called, such as removing the SCRIPT node that's been added to the DOM, and deleting the bridge method, since they are no longer used once you have the result.
