# User Interface #

### Really nice DOM programming! ###
An all-new [DOM package](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/dom/client/package-summary.html) models the W3C HTML Specification for DOM Elements. These new classes allow you to do elegant DOM programming using per-element methods rather than the static DOM helper methods required in GWT 1.4. Best of all, there is no extra memory or speed overhead! The GWT compiler's new support for JavaScriptObject is designed to allow comppletely cost-free JS interoperation.

Note that `com.google.gwt.dom.client.user.client.Element` now extends `dom.client.Element`. `dom.client.Element` is the preferred class to use for new code, and `user.client.Element` exists only for backwards compatibility. `UIObject` now generally uses the `dom.client.Element` instead of `user.client.Element`, although `setElement(user.client.Element)` and `getStyleElement()` have been preserved for backwards compatibility.

### New sample: Showcase ###
The new Showcase example replaces KitchenSink as the all-inclusive demo of GWT widgets and features. Examples of almost all Widgets can be found in the Showcase, along with source code and style definitions that will allow users to copy the examples easily.  The Showcase illustrates locale support (including Arabic, a right-to-left language), animations, and all of the new style themes.

### Default visual themes ###
GWT now contains three optional visual themes that give widgets a pleasant default look and feel: Standard, Chrome, and Dark. The Standard theme uses subtle shades of blue paired with rounded corners, to give a simple, clean appearance that works well for many apps. The Chrome theme is similar to Standard but includes gray-scale gradients to give emphasize a minimalist visual style. The Dark theme uses dark colors and hard edges for a bold, unusual appearance. The Standard theme is used by default in GWT applications created with `applicationCreator`, but users can select a different theme by inheriting an alternative theme module in their module XML.

Please note that, while these themes may serve as a useful starting point, they do not limit you in any way. You don't need to use them at all if you don't want to, and most applications will need to make significant additions or changes to the CSS introduced by these themes. The new Showcase sample lets you interactively explore the CSS styles associated with various widgets so that you can develop an intuition about how to use CSS to create exactly the style you want.

### Widget animations ###
Several widgets have optional animations to enhance user feedback: `Tree`, `TabPanel`, `DisclosurePanel`, `PopupPanel`, and `MenuBar`. You can enable an animation on any Widget that supports animations by calling `someWidget.setAnimationEnabled(true)`.

### Standards mode support ###
GWT now supports HTML "standards mode" in addition to each browser's traditional "quirks mode". As of 1.5, `applicationCreator` generates HTML that uses standards mode. Note that in future GWT releases, support for quirks mode will be de-prioritized; new features and bugs will focus on perfecting standards mode behavior. When practical, you should switch your HTML to standards mode by including a tag like this at the top of your html file:
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

Please note: There is a known issue with the StackPanel when using standards mode in IE in that makes it difficult to set the height of a StackPanel reliably. This is an issue that is targeted for priority attention for the next release of GWT.

### Bi-di support ###
As demonstrated in the new Showcase sample, GWT now has significant support for right-to-left languages. If the active locale is a right-to-left language, the dir="rtl" attribute is added to the 

&lt;html&gt;

 element on the first call to `RootPanel.get(id)` or `RootPanel.get()`.

Specifics:
  * User.gwt.xml now inherits the I18N module because standard widgets now support bidi.
  * Added `HasDirection` interface. This interface allows those widgets that implement it to set their own directionality, thereby overriding the document's global directionality. This interface should only be implemented by leaf widgets (i.e. not by widgets that can contain other widgets). Curently, `TextArea`, `TextBox`, and `Label` implement this interface.
  * Added utility class `com.google.gwt.i18n.client.BidiUtils`, which is used to set/get the directionality of an element.
  * The default alignment of the `HorizontalPanel` is now "left" in LTR layout, and "right" in RTL layout.
  * `DockPanel` defines two new direction constants - LINE\_START and LINE\_END. In an RTL environment, LINE\_START corresponds to the right side of the `DockPanel`. In an LTR environment, LINE\_START corresponds to the left side of the `DockPanel`.
  * Tree is now bidi-compatible. Branches expand to the left when the layout is RTL. Keyboard navigation is now bidi-compatible: in an RTL layout, the left key expands branches, and the right key collapses them.
  * Menu is now bidi-compatible. Submenus pop out to the left in an RTL layout. Keyboard navigation has been changed so that the left key expands submenus in an RTL layout.
  * `HorizontalSplitPanel` is now bidi-compatible. Two methods have been introduced to support adding widgets to panels positioned at the "start of the line layout", and the "end of the line layout". `HorizontalSplitPanel.add()` has been changed so that widgets are added in line layout order. For example, in RTL layout, the first widget that is added via `HorizontalSplitPanel.add()` will be placed in the right pane, and the second will be placed in the left pane. `setSplitPosition()` still sets the size of the left pane in RTL mode.
  * Added `TreeImagesRTL` and `DisclosureImagesRTL`, which are used to supply the widget images in RTL layout.
  * Modified `MenuBar` so that it accepts an `ImageBundle` as input to its constructor for its images. The default `ImageBundle` is `MenuImages` in LTR layout, and `MenuImagesRTL` in RTL layout.

### Accessibility support ###
GWT 1.5 adds `com.google.gwt.user.client.ui.Accessibility` for setting [ARIA](http://developer.mozilla.org/en/docs/Accessible_DHTML) roles/states. GWT standard widgets now support ARIA roles/states, including `CustomButton`, `Tree`, `TreeItem`, `MenuBar`, `MenuItem`, `TabBar`, `TabPanel`. ARIA support has been tested with Firefox 3 and Window-Eyes. There is some support for [FireVox](http://firevox.clcworld.net/), but results may vary; we plan to improve FireVox support in the future.

Specifics:
  * Tab indexes are properly set for all widgets that derive from `FocusWidget`.
  * A tabindex of -1 is set on the GWT history and script IFRAMES so that they are not part of the tab cycle.
  * Added `Document.createUniqueId()` as a general-purpose method for generating unique DOM ids.
