# How can I efficiently handle events from many interior Widgets? #

Quirksmode has a writeup of the background behind [Event Bubbling](http://www.quirksmode.org/js/events_order.html) in how different browsers deliver events.  When using a GWT event handler, GWT hides all this away from you and delivers events only for the element you are interested in.  This is a wonderful programming abstraction, but can lead to slow performance when many handlers are added to the system.

If you are building a widget, you may want to consider handling events in a different way, called Event Bubbling.  Essentially, this technique means you have one handler for the entire widget, and you have one handler callback that executes on behalf of all interior widgets.

See the [implementation of the Tree](http://google-web-toolkit.googlecode.com/svn/releases/1.5/user/src/com/google/gwt/user/client/ui/Tree.java) class for an example of using Event Bubbling in GWT.
