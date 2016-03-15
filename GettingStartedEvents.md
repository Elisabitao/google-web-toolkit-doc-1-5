# Add event listeners #

Like many user interface frameworks, GWT is event-based.  This means that all code executes in response to some other _event_ occurring.  Often, that event is triggered by the user, who uses the mouse or keyboard to interact with the application's interface.

## Listener interfaces ##

Before you can respond to an event you'll first need to tell GWT which types of events you're interested in.  This is known as _subscribing_ to events.  To subscribe to an event, you'll pass a particular _listener interface_ to the appropriate widget.  The listener interface defines one or more methods that the widget can later call to announce an event when it occurs.  From the widget point of view, we say that the widget _publishes_ the events that it can forward on to any listeners.

## Subscribing to events ##

There are a number of different listener interfaces available in GWT.  For example, the [ClickListener](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/ClickListener.html) interface is a simple event listener for click events.  It contains just one method, [onClick(Widget)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/ClickListener.html#onClick(com.google.gwt.user.client.ui.Widget)).  That method will be called whenever the user clicks on a widget that publishes click events.  One such widget is the [Button](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Button.html) class.  To see this in action, let's return to our StockWatcher example.  We already have a Button for adding stocks to the list.  Now we will handle its click event by passing it a ClickListener interface.

For this, we'll need the aptly named [addClickListener(ClickListener)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/FocusWidget.html#addClickListener(com.google.gwt.user.client.ui.ClickListener)).  Let's go ahead and call that method within our onModuleLoad():

```
public void onModuleLoad() {
  // set up stock list watch list
  stocksFlexTable.setText(0, 0, "Symbol");
  stocksFlexTable.setText(0, 1, "Price");
  stocksFlexTable.setText(0, 2, "Change");
  stocksFlexTable.setText(0, 3, "Remove");
  
  // set up event listeners for adding a new stock
  addButton.addClickListener(new ClickListener() {
    public void onClick(Widget sender) {
      addStock();
    }
  });

  // assemble Add Stock panel
  addPanel.add(newSymbolTextBox);
  addPanel.add(addButton);
  
  // assemble main panel
  mainPanel.add(stocksFlexTable);
  mainPanel.add(addPanel);
  mainPanel.add(lastUpdatedLabel);
  
  // add the main panel to the HTML element with the id "stockList"
  RootPanel.get("stockList").add(mainPanel);
    
  // move cursor focus to the text box
  newSymbolTextBox.setFocus(true);
}

private void addStock() {
  // executed when user clicks the addButton
}
```

You'll also need to add a couple of new `import` statements to the top of `StockWatcher.java`:

```
import com.google.gwt.user.client.ui.ClickListener;
import com.google.gwt.user.client.ui.Widget;
```

You'll notice that we used an anonymous inner class to implement [ClickListener](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/ClickListener.html).  In it we defined the [onClick(Widget)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/ClickListener.html#onClick(com.google.gwt.user.client.ui.Widget)) method, which delegates to a new method, addStock().  That's where we'll eventually do the work of validating the stock symbol and adding it to the watch list.

For smaller applications that only handle a relatively small number of events, using anonymous inner classes gets the job done with minimal fuss.  However, if we were subscribing to events from a large number of widgets this approach can be inefficient, since it could result in the creation of many separate listener objects.  In that case, it's better to have the containing class implement the listener interface and use a single listener method for events coming from multiple event publishers.  Widgets supply their `this` pointer as the `sender` parameter when they invoke the method, so you'll know which one triggered the event. This makes better use of memory but requires slightly more code, as shown in the following example:

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

## Listener adapters ##

Now, back to StockWatcher.  Once we implement addStock() in the next section the user will be able to click the Add button to add a stock to the list.  For convenience, let's also allow him to use the keyboard and hit Enter from inside `newSymbolTextBox` to add it.  To do this, we'll subscribe to `newSymbolTextBox`'s keyboard events by passing a [KeyboardListener](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/KeyboardListener.html) to its [addKeyboardListener(KeyboardListener)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/FocusWidget.html#addKeyboardListener(com.google.gwt.user.client.ui.KeyboardListener)) method.

Looking at the KeyboardListener interface, we see that it contains three listener methods:

  * [onKeyDown(Widget, char, int)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/KeyboardListenerAdapter.html#onKeyDown(com.google.gwt.user.client.ui.Widget,%20char,%20int))  Fired when the user depresses a physical key.
  * [onKeyPress(Widget, char, int)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/KeyboardListenerAdapter.html#onKeyPress(com.google.gwt.user.client.ui.Widget,%20char,%20int))  Fired when a keyboard action generates a character.
  * [onKeyUp(Widget, char, int)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/KeyboardListenerAdapter.html#onKeyUp(com.google.gwt.user.client.ui.Widget,%20char,%20int))  Fired when the user releases a physical key.

For our example this seems like overkill: we really only need the [onKeyDown(Widget, char, int)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/KeyboardListenerAdapter.html#onKeyDown(com.google.gwt.user.client.ui.Widget,%20char,%20int)) listener method.  What we need is a class that allows us to handle the specific events we're interested in and ignore the rest.  As it turns out, GWT has _adapter_ classes for just that purpose.  Adapters are simply empty concrete implementations of a particular event interface.  Just subclass an adapter class and provide implementations for the events you want to subscribe to.

For StockWatcher, we'll subclass the [KeyboardListenerAdapter](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/KeyboardListenerAdapter.html) interface and override the [onKeyDown(Widget, char, int)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/KeyboardListenerAdapter.html#onKeyDown(com.google.gwt.user.client.ui.Widget,%20char,%20int)) listener method.  Add the import:

```
import com.google.gwt.user.client.ui.KeyboardListenerAdapter;
```

Then add the call to [addKeyboardListener(KeyboardListener)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/FocusWidget.html#addKeyboardListener(com.google.gwt.user.client.ui.KeyboardListener)) in onModuleLoad() as follows:

```
// set up event listeners for adding a new stock
addButton.addClickListener(new ClickListener() {
  public void onClick(Widget sender) {
    addStock();
  }
});

newSymbolTextBox.addKeyboardListener(new KeyboardListenerAdapter() {
  @Override
  public void onKeyDown(Widget sender, char keyCode, int modifiers) {
    if (keyCode == KEY_ENTER) {
      addStock();
    }
  }
});

// assemble Add Stock panel
addPanel.add(newSymbolTextBox);
addPanel.add(addButton);
```

Our event listeners are now wired up and ready to go.  The next step will be to fill out our empty addStock() method to actually add the stock to our watch list when our handled events occur.
