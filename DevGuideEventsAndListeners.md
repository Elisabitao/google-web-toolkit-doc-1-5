# Events and Listeners #

Events in GWT use the _listener interface_ model similar to other user interface frameworks. A listener interface defines one or more methods that the widget calls to announce an event. A class wishing to receive events of a particular type implements the associated listener interface and then passes a reference to itself to the widget to _subscribe_ to a set of events. The [Button](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Button.html) class, for example, publishes click events. The associated listener interface is [ClickListener](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/ClickListener.html).

The following example demonstrates how to add a custom ClickListener subclass to an instance of a Button:

```
public void anonClickListenerExample() {
  Button b = new Button("Click Me");
  b.addClickListener(new ClickListener() {
    public void onClick(Widget sender) {
      // handle the click event
    }
  });
}
```

Using anonymous inner classes as in the above example can use excessive memory for a large number of widgets, since it could result in the creation of many listener objects. Instead of creating separate instances of the ClickListener object for each widget that needs to be listened to, a single listener can be shared between many widgets. Widgets supply their `this` pointer as the `sender` parameter when they invoke a listener method, allowing a single listener to distinguish between multiple event publishers. This makes better use of memory but requires slightly more code, as shown in the following example:

```
public class ListenerExample extends Composite implements ClickListener {
  private FlowPanel fp = new FlowPanel();
  private Button b1 = new Button("Button 1");
  private Button b2 = new Button("Button 2");

  public ListenerExample() {
    initWidget(fp);
    fp.add(b1);
    fp.add(b2);
    b1.addClickListener(this);
    b2.addClickListener(this);
  }

  public void onClick(Widget sender) {
    if (sender == b1) {
      // handle b1 being clicked
    } else if (sender == b2) {
      // handle b2 being clicked
    }
  }
}
```

Some event interfaces specify more than one event. If you are only interested in a subset of these events, subclass one of the event _adapters_. Adapters are simply empty concrete implementations of a particular event interface, from which you can derive a listener class without having to implement every method.

```
public void adapterExample() {
  TextBox t = new TextBox();
  t.addKeyboardListener(new KeyboardListenerAdapter() {
    public void onKeyPress(Widget sender, char keyCode, int modifiers) {
      // handle only this one event
    }
  });
}
```

