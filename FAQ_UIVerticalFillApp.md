# How do I create an app that fills the page vertically when the browser window resizes? #
Creating an application that fills the browser window can be a challenge for web application designers. The underlying problem is that different browsers treat the CSS specification of "100%" for height in different ways.

The [Mail sample application](http://gwt.google.com/samples/Mail/Mail.html) demonstrates a simple cross-browser technique using GWT.

  1. Instead of setting the CSS height attribute to "100%", use absolute pixel ("px") units to set the height of the outermost widget.
  1. Then, add a window resize handler in order to reset the height whenever the browser window changes size.
```
   final VerticalPanel vp = new VerticalPanel();
   vp.add(mainPanel);
   vp.setWidth("100%");
   vp.setHeight(Window.getClientHeight() + "px");
   Window.addWindowResizeListener(new WindowResizeListener() {

     public void onWindowResized(int width, int height) {
       vp.setHeight(height + "px");
     }
   });
   RootPanel.get().add(vp);
```

The above code snippet has been tested to work on Internet Explorer, Safari, and Firefox.

**Important:** The type of panel is significant. Some GWT panels are generated as HTML tables, others as HTML divs. Browsers render height attributes set as a percentage differently if an element is nested in a table rather than nested in a div. Therefore, if you decide to change the outer widget to a different type of panel or widget, make sure you test your application on all of your target browsers.
