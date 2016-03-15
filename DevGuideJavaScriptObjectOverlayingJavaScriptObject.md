The JavaScriptObject overlay (JSO overlay) facility is a powerful technique that allows you to directly overlay JavaScript objects from the wild with a strongly-typed Java class. This gives you the benefit of easily interoperating with external JavaScript objects within your GWT. Since you would now be working with a Java class, this technique also allows you to leverage the GWT compiler optimizations in finding and optimizing usage of the JSO overlay subclass.

This JSO overlay technique uses JavaScript Native Interface (JSNI) methods, so if you're unfamiliar with JSNI, you may want to read up on that first to understand its usage here.

Now let's get into an example!

Let's start with something simple. Suppose we have some arbitrary JavaScript object called 'account' that we want to overlay and that defines a 'firstName' and 'lastName' property. In overlaying this object, we would want to have methods available to get and set this object's properties.

The corresponding JSO overlay subclass would look something like the following:

```
public class Account extends JavaScriptObject {

  private final native static <T extends JavaScriptObject> T fromJson(String json) /*-{
    return $wnd.eval("(" + json + ")");
  }-*/;

  public final native String getFirstName() /*-{
    return this.firstName;
  }-*/;

  public final native String getLastName() /*-{
    return this.lastName;
  }-*/;

  public final native void setFirstName(String firstName) /*-{
    this.firstName = firstName;
  }-*/;

  public final native void setLastName(String lastName) /-*{
    this.lastName = lastName;
  }-*/;
}
```

Suppose you wanted to use a JSO subclass to overlay a regular JavaScript array. A JavaScript array is nothing more than an array data structure that allows you to retrieve elements by index and a `length` property that maintains how many element are contained within the array. If we were to overlay a JavaScript array from the JS world, these are two properties that we'd want to be able to access from the JSO overlay object overlaying the JS array.
