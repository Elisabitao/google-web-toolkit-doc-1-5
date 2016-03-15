# Cross-browser Support #

GWT shields you from worrying too much about cross-browser incompatibilities. If you stick to built-in [widgets](DevGuideWidgetsAndPanels.md) and [composites](DevGuideCreatingCustomWidgets.md), your applications will work similarly on the most recent versions of Internet Explorer, Firefox, and Safari. (Opera, too, most of the time.) DHTML user interfaces are remarkably quirky, though, so make sure to test your applications thoroughly on every browser.

Whenever possible, GWT defers to browsers' native user interface elements. For example, GWT's [Button](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Button.html) widget is a true HTML `<button>` rather than a synthetic button-like widget built, say, from a `<div>`. That means that GWT buttons render appropriately in different browsers and on different client operating systems. We like the native browser controls because they're fast, accessible, and most familiar to users.


When it comes to styling web applications, [CSS](http://www.w3.org/Style/CSS/) is ideal. So, instead of attempting to encapsulate UI styling behind a wall of least-common-denominator APIs, GWT provides very few methods directly related to style. Rather, developers are encouraged to define styles in stylesheets that are linked to application code using [style names](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#setStyleName(java.lang.String)). In addition to cleanly separating style from application logic, this division of labor helps applications load and render more quickly, consume less memory, and even makes them easier to tweak during edit/debug cycles since there's no need to recompile for style tweaks.


> _Tip: If you find a need to implement a browser specific dependency, you can use a [JSNI](DevGuideJavaScriptNativeInterface.md) method to retrieve the browser' UserAgent string._

```
  public static native String getUserAgent() /*-{
     return navigator.userAgent.toLowerCase();
  }-*/
```
> 


### See Also ###
  * [Style Sheets](DevGuideStyleSheets.md)
