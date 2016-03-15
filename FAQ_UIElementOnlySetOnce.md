<a href='Hidden comment: 
2008-11-27. Deprecated. Duplicate of FAQ_AssertionElementMayOnlyBeSetOnce. Combined content.
'></a>
# Why do I get the assertion, "Element may only be set once?" #

You will see the "Element may only be set once" error because trying to call the setElement() method on a widget after it's already been initialized is illegal. This assertion was loosened in GWT 1.4 but is back in GWT 1.5.  Setting a Widget's element to an arbitrary new element would break all sorts of assumptions made by most widgets.

If you are trying to create a UI component out of an existing DOM element, you can try subclassing [AbsolutePanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/AbsolutePanel.html) and use the protected [AbsolutePanel(Element)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/AbsolutePanel.html#AbsolutePanel(com.google.gwt.user.client.Element)) constructor to change the underlying element.
