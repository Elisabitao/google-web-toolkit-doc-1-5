# Accessing the Browser's DOM #

Browsers provide an interface to examine and manipulate the on-screen elements using the [DOM](http://w3c.org/DOM/) (Document Object Model).  Traditionally, JavaScript programmers use the DOM to program the user interface portion of their logic, and traditionally, they have had to account for the many differences in the implementation of the DOM on different browsers.

So that you don't have to worry (generally) about cross-browser support when implementing user interfaces, GWT provides a set of [widget and panel](DevGuideWidgetsAndPanels.md) classes that wrap up this functionality. But sometimes you need to access the DOM. For example, if you want to:

  * provide a feature in your user interface that GWT does not support
  * write a new Widget class
  * access an HTML element defined directly in the host page
  * handle browser Events at a low level
  * perform some filtering or other processing on an HTML document loaded into the browser

You could use [JSNI](DevGuideJavaScriptNativeInterface.md) methods to access the browser's DOM, but GWT provides a convenience [DOM](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/DOM.html) class.   The DOM class is not meant to be instantiated. Instead it is used as a collection of static methods that provides a way to walk and query the tree, plus a number of convenience routines. The DOM class also provides a layer of cross-browser support.

## Using the DOM to manipulate a widget ##

Each widget and panel has an underlying DOM element that you can access with the [getElement()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#getElement()) method. You can use the getElement() method to get the underlying element from the DOM and manipulate or apply attributes to it with the DOM class.

The following example shows how to set a style attribute to change a widget's background color.

```
private HTML htmlWidget;

// Other code to instantiate the widget...

// Change the description background color.
DOM.setStyleAttribute(htmlWidget.getElement(), "backgroundColor", "#fffe80");
```

Here, the getElement() method derived from the `Widget` superclass returns a DOM [Element](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/Element.html) object representing a node in the DOM tree structure and adds a style attribute to it.

This is an example where using the DOM isn't absolutely necessary. An alternative approach is to use  [style sheets](DevGuideStyleSheets.md) and associate different style classes to the widget using the [setStylePrimaryName()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#setStylePrimaryName(java.lang.String))  or [setStyleName()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#setStyleName(java.lang.String)) method instead.

## Finding an element in the DOM ##

The following example shows how to combine a JSNI method with Java code to manipulate the DOM.  First, is a JSNI routine that will retrieve all the child elements that are Anchor tags. The element objects are assigned a unique ID for easy access from Java:

```
/**
 * Find all child elements that are anchor tags,
 * assign a unique id to them, and return a list of
 * the unique ids to the caller.
 */
 private native void putElementLinkIDsInList(Element elt,
                                                ArrayList list) /*-{
      var links = elt.getElementsByTagName("a");

      for (var i = 0; i < links.length; i++ ) {
        var link = links.item(i);
        link.id = ("uid-a-" + i);
        list.@java.util.ArrayList::add(Ljava/lang/Object;) (link.id);
      }
    }-*/;
```


And what could you possibly do with a DOM element once you have found it?  This code iterates through all the anchor tags returned from the above method and then rewrites where it points to:

```
/**
 * Find all anchor tags and if any point outside the site, 
 * redirect them to a "blocked" page.
 */
 private void rewriteLinksIterative() {
   ArrayList links = new ArrayList();
   putElementLinkIDsInList(this.getElement(), links);
   for (int i = 0; i < links.size(); i++) {
     Element elt = DOM.getElementById((String) links.get(i));
     rewriteLink(elt, "www.example.com");
   }
 }

/**
 * Block all accesses out of the website that don't match 'sitename'
 * @param element An anchor link element
 * @param sitename name of the website to check.  e.g. "www.example.com"
 */
 private void rewriteLink(Element element, sitename) {
   String href = DOM.getElementProperty(element, "href");

   if (null == href) {
       return;
   }

   // We want to re-write absolute URLs that go outside of this site
   if (href.startsWith("http://") &&
        !href.startsWith("http://"+sitename+"/") {
       DOM.setElementProperty(element, "href", 
           "http://"+sitename+"/Blocked.html" );
   }
 }
```

The JSNI method set an ID on each element which we then used as an argument to [DOM.getElementById(id)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/DOM.html#getElementById(java.lang.String)) to fetch the opaque `Element` handle in Java.  Then the DOM static methods can be used to query and modify the properties of that element.

## Using the DOM to capture a browser event ##

GWT contains an [Event](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/Event.html) class  as an opaque handle to a native DOM Event. This object can be passed back and forth to JavaScript through [JSNI](DevGuideJavaScriptNativeInterface.md) methods. It can also be manipulated by the GWT DOM class.

This example shows how to use the DOM methods to catch a keyboard event for particular elements and handle them before the [event](DevGuideEventsAndListeners.md) gets dispatched:

```
  private ArrayList keyboardEventReceivers = new ArrayList();

  /**
   * Widgets can register their DOM element object if
   * they would like to be a trigger to intercept keyboard events
   */
  public void registerForKeyboardEvents (Element  e) {
    this.keyboardEventReceivers.add(e);
  }

  /**
   * Returns true if this is one of the keys we are interested in
   */  
  public boolean isInterestingKeycode (int keycode) {
     //...
  }

  /**
   * Setup the event preview class when the module is loaded.
   */
  private void setupKeyboardShortcuts() {

    // Define an inner class to handle the event
    DOM.addEventPreview(new EventPreview() {
      public boolean onEventPreview(Event event) {
        
          Element elt = DOM.eventGetTarget(event);
          int keycode = DOM.eventGetKeyCode(event);
          boolean ctrl = DOM.eventGetCtrlKey(event);
          boolean shift = DOM.eventGetShiftKey(event);
          boolean alt = DOM.eventGetAltKey(event);
          boolean meta = DOM.eventGetMetaKey(event);
          
          if (DOM.eventGetType(event) != Event.ONKEYPRESS
              || ctrl || shift || alt || meta
              || keyboardEventReceivers.contains(elt)
              || !isInterestingKeycode(keycode)) {
            // Tell the event handler to continue processing this event.
            return true;
          }
         
          GWT.log("Processing Keycode" + keycode, null );
          handleKeycode(keycode);
          // Tell the event handler that this event has been consumed
          return false;
      }});
  }

  /**
   * Perform the keycode specific processing
   */
  private void handleKeycode (int keycode) {
    switch (keycode) {
        //...
    }
  }
```
