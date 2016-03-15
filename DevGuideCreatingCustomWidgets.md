# Creating Custom Widgets #

GWT makes it easy to create custom user interface elements.  There are three general strategies to follow:

  * Create a widget that is a composite of existing widgets.
  * Create an entirely new widget written in the Java language.
  * Create a widget that wraps JavaScript using [JSNI](DevGuideJavaScriptNativeInterface.md) methods.

There are numerous third party libraries that provide widgets you can integrate into your GWT module that were created using the strategies listed above.

## Building Composites ##
The most effective way to create new widgets is to extend the [Composite](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Composite.html) class. A composite is a specialized widget that can contain another component (typically, a [panel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Panel.html)) but behaves as if it were its contained widget. You can easily combine groups of existing widgets into a composite that is itself a reusable widget. Some of the UI components provided in GWT are composites: for example, the [TabPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TabPanel.html) (a composite of a TabBar and a DeckPanel) and the [SuggestBox](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/SuggestBox.html).

Rather than create complex widgets by subclassing [Panel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Panel.html) or another [Widget](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Widget.html) type, it's better to create a composite because a composite usually wants to control which methods are publicly accessible without exposing those methods that it would inherit from its Panel superclass.

### Example Composite Widget ###
The following code snippet shows how to create a composite widget composed of a [TextBox](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TextBox.html) widget and a [CheckBox](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/CheckBox.html) widget laid out in a [VerticalPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/VerticalPanel.html).

```
package com.google.gwt.examples;

import com.google.gwt.core.client.EntryPoint;
import com.google.gwt.user.client.ui.CheckBox;
import com.google.gwt.user.client.ui.ClickListener;
import com.google.gwt.user.client.ui.Composite;
import com.google.gwt.user.client.ui.RootPanel;
import com.google.gwt.user.client.ui.TextBox;
import com.google.gwt.user.client.ui.VerticalPanel;
import com.google.gwt.user.client.ui.Widget;

public class CompositeExample implements EntryPoint {

  /**
   * A composite of a TextBox and a CheckBox that optionally enables it.
   */
  private static class OptionalTextBox extends Composite implements
      ClickListener {

    private TextBox textBox = new TextBox();
    private CheckBox checkBox = new CheckBox();

    /**
     * Constructs an OptionalTextBox with the given caption on the check.
     * 
     * @param caption the caption to be displayed with the check box
     */
    public OptionalTextBox(String caption) {
      // Place the check above the text box using a vertical panel.
      VerticalPanel panel = new VerticalPanel();
      panel.add(checkBox);
      panel.add(textBox);

      // Set the check box's caption, and check it by default.
      checkBox.setText(caption);
      checkBox.setChecked(true);
      checkBox.addClickListener(this);

      // All composites must call initWidget() in their constructors.
      initWidget(panel);

      // Give the overall composite a style name.
      setStyleName("example-OptionalCheckBox");
    }

    public void onClick(Widget sender) {
      if (sender == checkBox) {
        // When the check box is clicked, update the text box's enabled state.
        textBox.setEnabled(checkBox.isChecked());
      }
    }

    /**
     * Sets the caption associated with the check box.
     * 
     * @param caption the check box's caption
     */
    public void setCaption(String caption) {
      // Note how we use the use composition of the contained widgets to provide
      // only the methods that we want to.
      checkBox.setText(caption);
    }

    /**
     * Gets the caption associated with the check box.
     * 
     * @return the check box's caption
     */
    public String getCaption() {
      return checkBox.getText();
    }
  }

  public void onModuleLoad() {
    // Create an optional text box and add it to the root panel.
    OptionalTextBox otb = new OptionalTextBox("Check this to enable me");
    RootPanel.get().add(otb);
  }
}
```


## From Scratch in Java Code ##

It is also possible to create a widget from scratch, although it is trickier because you have to write code at a lower level. Many of the basic GWT widgets are written this way, such as [Button](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Button.html) and [TextBox](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TextBox.html).

To understand how to create your own, refer to the implementations of these widgets in the com.google.gwt.user.client.ui package. The source code is in gwt-user.jar.

## Using JavaScript ##

When implementing a custom widget that derives directly from the [Widget](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Widget.html) class, you may also write some of the widget's methods using JavaScript. This should generally only be done as a last resort, as it becomes necessary to consider the cross-browser implications of the native methods that you write, and also becomes more difficult to debug. For an example of this pattern in practice, see the [TextBox](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TextBox.html) widget and the underlying JavaScript implementation of some of its methods in the TextBoxImpl class. You should use [deferred binding](DevGuideDeferredBinding.md) to isolate browser-specific code.
