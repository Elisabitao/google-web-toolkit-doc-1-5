# AssertionError: Element may only be set once #
After upgrading to GWT 1.5, you might see this exception:

```
$PP_OFF
[ERROR] Uncaught exception escaped
java.lang.AssertionError: Element may only be set once
```

The message indicates that some widget is calling the setElement() method more than once. Trying to call the setElement() method on a widget after it's already been initialized is illegal.

This assertion was loosened in GWT 1.4 but is back in GWT 1.5. Setting a widget's element to an arbitrary new element would break all sorts of assumptions made by most widgets; that is, it makes it impossible for a widget to make any assumptions about the stability of its underlying element. If the element can change arbitrarily, all kinds of things can go wrong.

Calling the setElement() method more than once almost always happens unintentionally (for example, extending a widget that calls setElement() in its constructor then calling it again from the subclass constructor). Thus the message generally reveals an actual problem that was going unnoticed.

### Workaround ###
Rather than write a new widget from scratch, here's a shortcut for constructing a widget from an existing DOM element.

First subclass [AbsolutePanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/AbsolutePanel.html). Then use the protected [AbsolutePanel(Element)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/AbsolutePanel.html#AbsolutePanel(com.google.gwt.user.client.Element)) constructor to change the underlying element.
