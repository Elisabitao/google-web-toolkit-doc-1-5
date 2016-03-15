# Defining Deferred Binding Rules #

There are two techniques for implementing deferred binding:

  * [Replacement](DevGuideDeferredBindingReplacement.md): One class is replaced with another depending on a condition.
  * [Generators](DevGuideDeferredBindingGenerators.md): A set of derived classes is automaticaly generated and subsequently replaced depending on a condition.


## Directives in Module XML files ##

The deferred binding mechanism is completely configurable and does not require editing the GWT distributed source code.  Deferred binding is configured through the `<replace-with>` and `<generate-with>` elements in the [module XML files](DevGuideModuleXml.md).  The deferred binding rules are pulled into the module build through `<inherits>` elements.

For example, the following configuration invokes deferred binding for the  [PopupPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/PopupPanel.html) widget:

  * Top level 

&lt;module&gt;

_.gwt.xml_**inherits**_[com.google.gwt.user.User](http://google-web-toolkit.googlecode.com/svn/trunk/user/src/com/google/gwt/user/User.gwt.xml)
  * [com/google/gwt/user/User.gwt.xml](http://google-web-toolkit.googlecode.com/svn/trunk/user/src/com/google/gwt/user/User.gwt.xml)_**inherits**_[com.google.gwt.user.Popup](http://google-web-toolkit.googlecode.com/svn/trunk/user/src/com/google/gwt/user/Popup.gwt.xml)
  * [com/google/gwt/user/Popup.gwt.xml](http://google-web-toolkit.googlecode.com/svn/trunk/user/src/com/google/gwt/user/Popup.gwt.xml)_**contains**_`<replace-with>` elements to define deferred binding rules for the [PopupPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/PopupPanel.html) class._

Inside the [PopupPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/PopupPanel.html) module XML file, there happens to be some rules defined for deferred binding. In this case, we're using the [Replacement](DevGuideDeferredBindingReplacement.md) rule.
