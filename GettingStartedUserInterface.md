# Create the user interface #

At this point we've decided what StockWatcher is going to do and we've discussed how to go about writing translateable Java source for the GWT compiler. We'll start building the application with the user interface.

GWT user interfaces are composed of [widgets](DevGuideWidgetGallery.md) and [panels](DevGuideUnderstandingLayout.md). Widgets provide models for commonly-used interface elements like [buttons](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Button.html), [text boxes](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TextBox.html), and [trees](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Tree.html). Panels, such as [DockPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/DockPanel.html), [HorizontalPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HorizontalPanel.html), and [RootPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/RootPanel.html), contain widgets and are used to define [how they are laid out](DevGuideUnderstandingLayout.md) in the browser.

Widgets and panels work the same way on all browsers. By using them, you eliminate the need to write specialized code for each browser. GWT has a full complement of widgets available "out of the box," but you are not limited to the set of widgets provided by the toolkit. There are a number of ways to [create custom widgets](DevGuideCreatingCustomWidgets.md) yourself. You can also find a good selection of [third-party widget libraries](http://code.google.com/hosting/search?q=gwt&projectsearch=Search+Projects) online.

## RootPanel ##

At the top of any GWT user interface hierarchy, there is always a [RootPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/RootPanel.html). A RootPanel always wraps an actual element in the [HTML host page](DevGuideHostPage.md). The default RootPanel wraps the HTML host page's `<body>` element.  You can get a reference to this RootPanel by calling [RootPanel.get()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/RootPanel.html#get). You can also get a RootPanel wrapping other elements in the host page. Just give those elements `id` attributes and then pass the ID to [RootPanel.get(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/RootPanel.html#get).

Thus, you have two options for constructing your GWT application's interface.  You can construct parts of your application's interface using regular static HTML and just insert GWT widgets and panels into named placeholder elements. This option is especially useful for embedding GWT into an existing application. Alternatively, your host page can contain an empty `<body>`, in which case you'll get a reference to the default RootPanel and then build up the entire interface with GWT [widgets](DevGuideWidgetGallery.md).

For StockWatcher, we're going to use placeholder elements in our HTML.  Go ahead and open the host page file (`src/com/google/gwt/sample/stockwatcher/public/StockWatcher.html`) and replace its contents with the following HTML:

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">

<html>
  <head>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <title>StockWatcher</title>
    <script type="text/javascript" language="javascript" src="com.google.gwt.sample.stockwatcher.StockWatcher.nocache.js"></script>
  </head>

  <body>
    <h1>Stock Watcher</h1>
    <div id="stockList"></div>
  </body>
</html>

```

This code defines the static content of the page.  As in normal HTML, the `<title>` tag is just the title of our page that appears on the browser window or tab.  The `<script>` tag references the JavaScript source code that GWT will generate.  Finally, the `<div>` tag will contain our GWT widgets.  We set its `id` attribute so we can access it from GWT via a [RootPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/RootPanel.html).

In addition, the HTML 4.01 Transitional DOCTYPE declaration at the top of the file sets the rendering engine to "Standards Mode", which provides better cross browser compatibility.  If you remove this line, the page will be rendered in "Quirks Mode", which means that all of the old browser bugs will still be present.  In some cases, you may choose to use "Quirks Mode" if you are integrating with an application that relies on some of the browser specific bugs being present, but for now, we will stick with "Standards Mode".

## Panels and Widgets ##

Now we need to use GWT panels and widgets to construct the dynamic parts of the user interface.  At the [beginning](GettingStarted.md) of the Getting Started guide we showed you what the final StockWatcher application would look like. The stock watch list was displayed in the form of a table, with the new symbol textbox and Add button below, and the last update timestamp at the bottom. Since the elements of the UI are stacked vertically, we look through the [widget gallery](DevGuideWidgetGallery.md) and find that [VerticalPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/VerticalPanel.html) is just what we need. We'll use a GWT VerticalPanel with three children.

The first (top) child is the stock watch list itself. Since this is a table, we again turn to the gallery and find [HTMLTable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLTable.html), which looks promising. However, it is marked as `abstract` so we need to find a suitable subclass. [Grid](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Grid.html) won't work because it doesn't allow us to remove rows from the middle of the table (we'll need that feature for removing stocks from the list). [FlexTable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/FlexTable.html), on the other hand, _does_ have a [removeRow(int)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/FlexTable.html#removeRow) method.  It also has methods for setting the contents of any cell (identified by row and column index), automatically expanding the table if necessary.  That should come in handy.

The second child of our VerticalPanel will be the new symbol textbox and Add button. We'll want these two to be displayed on the same line, so we're going to need another nested panel to do the layout. For this, we'll use a [HorizontalPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HorizontalPanel.html) with a [TextBox](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TextBox.html) and [Button](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Button.html) as children.

Finally, the third child of our VerticalPanel will be the last update timestamp, which we'll display in a simple [Label](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Label.html). The Label widget is designed to display dynamic, non-HTML text.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherLayout.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherLayout.png)

Now then, to the code: open StockWatcher's entry point class (`src/com/google/gwt/sample/stockwatcher/client/StockWatcher.java`) and replace it with the following code:

```
package com.google.gwt.sample.stockwatcher.client;

import com.google.gwt.core.client.EntryPoint;
import com.google.gwt.user.client.ui.Button;
import com.google.gwt.user.client.ui.FlexTable;
import com.google.gwt.user.client.ui.HorizontalPanel;
import com.google.gwt.user.client.ui.Label;
import com.google.gwt.user.client.ui.RootPanel;
import com.google.gwt.user.client.ui.TextBox;
import com.google.gwt.user.client.ui.VerticalPanel;

public class StockWatcher implements EntryPoint {

  private VerticalPanel mainPanel = new VerticalPanel();   
  private FlexTable stocksFlexTable = new FlexTable();
  private HorizontalPanel addPanel = new HorizontalPanel();
  private TextBox newSymbolTextBox = new TextBox();
  private Button addButton = new Button("Add");
  private Label lastUpdatedLabel = new Label();

  public void onModuleLoad() {
    // set up stock list table
    stocksFlexTable.setText(0, 0, "Symbol");
    stocksFlexTable.setText(0, 1, "Price");
    stocksFlexTable.setText(0, 2, "Change");
    stocksFlexTable.setText(0, 3, "Remove");

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
}
```

What we're doing here is simply constructing the interface from GWT widets and panels. We're working bottom-up, first instantiating each widget/panel via class field initializers. Then in [onModuleLoad()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/EntryPoint.html#onModuleLoad()) (which as you may recall, is where we put our application's startup code), we set up the FlexTable's header row and then fill the panels with their respective children. The last step is to add our outer VerticalPanel to the RootPanel that wraps the `<div>` element named `stockList` in our HTML host page.

## Test the interface ##

It's time to test our changes.  Save the edited files and launch StockWatcher in hosted mode (click the _Run_ button in Eclipse, or run the `StockWatcher-shell` script if you're using another IDE).  You should see the our primitive form of StockWatcher appear in the hosted mode browser.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcher1.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcher1.png)

As you can see, our current iteration of StockWatcher isn't going to win any design awards.  That's ok.  We'll add some style later on with CSS.  What StockWatcher is really lacking is interactivity: the interface doesn't actually do anything... yet.  Let's fix that by adding some [event listeners](GettingStartedEvents.md).
