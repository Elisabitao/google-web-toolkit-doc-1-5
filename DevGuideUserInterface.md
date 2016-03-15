# Building User Interfaces #

GWT user interface classes are similar to those in existing UI frameworks such as [Swing](http://java.sun.com/j2se/1.4.2/docs/api/javax/swing/package-summary.html) and [SWT](http://www.eclipse.org/swt/) except that the widgets are rendered using dynamically-created HTML rather than pixel-oriented graphics.

In traditional JavaScript programming, dynamic user interface creation is done by manipulating the browser's DOM.  While GWT provides [access to the browser's DOM directly](DevGuideAccessingDOM.md) using the [DOM class](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/DOM.html), it is far easier to use classes from the [Widget](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Widget.html) hierarchy. The Widget classes make it easier to quickly build interfaces that will work correctly on all browsers.

## Specifics ##

[Widgets and Panels](DevGuideWidgetsAndPanels.md)
Widgets and panels are [client-side](DevGuideClientSide.md) Java classes used to build user interfaces.

[Widgets Gallery](DevGuideWidgetGallery.md)
A gallery of widgets and panels.

[Events and Listeners](DevGuideEventsAndListeners.md)
Widgets publish events using the well-known listener pattern.

[Understanding Layout](DevGuideUnderstandingLayout.md)
Understanding how widgets are laid out within panels.

[Style Sheets](DevGuideStyleSheets.md)
Widgets are most easily styled using cascading style sheets (CSS).

[Image Bundles](DevGuideImageBundles.md)
Optimize the performance of your application by reducing the number of HTTP requests for images.

[Creating Custom Widgets](DevGuideCreatingCustomWidgets.md)
Create your own widgets completely in Java code.

[Accessing the Browser's DOM](DevGuideAccessingDOM.md)
When necessary, GWT gives you the flexibility to manipulate the browser's DOM directly.

[Programming Delayed Logic](DevGuideDeferredCommand.md)
Classes that allow you to schedule activity to occur in the future.

[Date and Number Formatting](DevGuideDateAndNumberFormat.md)
Formatting Dates and Numbers including support for internationalization.

[Using the GWT Linker](DevGuideLinker.md)
The GWT Linker provides two built-in linker modules, and can be extended by users.
