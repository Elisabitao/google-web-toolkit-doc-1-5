# Add Styling #

Our beloved StockWatcher is _so close_ to being ready to launch.  We've implemented all the functionality: users can add stocks to their list and get continuous price updates.  We even tracked down a bug using [hosted mode](GettingStartedHostedMode.md).  So what's holding us back, now that it's feature-complete?  Well, the problem is that in its current form, StockWatcher is just a bit... how should we say this: _aesthetically-challenged_.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDebug3.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDebug3.png)

Ok.  Let's face the facts: StockWatcher is downright _ugly_.  But it doesn't have to stay that way.  Let's add some style and give our GWT application a much needed make-over.

## Using default visual themes ##

You'll notice that at least the buttons in StockWatcher have a fancy gradient as a background, but where did this come from?  GWT has three default visual themes that you can choose from: standard, chrome, and dark.  The standard theme uses subtle shades of blue to create an lively user interface.  The chrome theme uses gray scale backgrounds for a refined, professional look.  The dark theme uses dark shades of gray and black with bright indigo highlights for a bold, eye catching experience.

By default, new GWT applications use the standard theme, but you can select any one of the themes mentioned above.  Open the `StockWatcher.gwt.xml` file and uncomment the line that inherits the dark theme.  You'll also need to comment out the standard theme, as you should only use one visual theme at a time.

```
<module>

      <!-- Inherit the core Web Toolkit stuff.                        -->
      <inherits name='com.google.gwt.user.User'/>
	
      <!-- Inherit the default GWT style sheet.  You can change       -->
      <!-- the theme of your GWT application by uncommenting          -->
      <!-- any one of the following lines.                            -->
      <!-- <inherits name='com.google.gwt.user.theme.standard.Standard'/> -->
      <!-- <inherits name="com.google.gwt.user.theme.chrome.Chrome"/> -->
      <inherits name="com.google.gwt.user.theme.dark.Dark"/>

      <!-- Other module inherits                                      -->


      <!-- Specify the app entry point class.                         -->
      <entry-point class='com.google.gwt.sample.stockwatcher.client.StockWatcher'/>
    
      <!-- Specify the application specific style sheet.              -->
      <stylesheet src='StockWatcher.css' />
	
</module>
```

Restart your application in hosted mode (when you edit the `StockWatcher.gwt.xml` file, you need to restart hosted mode; refreshing in hosted mode will not pick up your changes), and your app should look very different.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDarkTheme.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDarkTheme.png)

GWT visual themes also come in RTL (right-to-left) versions if you are designing a website for a language that is writting right-to-left, such as arabic.  You can include the RTL version by adding `RTL` to the end of the module name:
```
  <inherits name="com.google.gwt.user.theme.dark.DarkRTL"/>
```

The rest of this tutorial uses the standard GWT visual theme.  If you've already edited the `StockWatcher.gwt.xml` file to try the dark theme, you should replace its contents with the following to use the standard theme.

```
<module>

      <!-- Inherit the core Web Toolkit stuff.                        -->
      <inherits name='com.google.gwt.user.User'/>
	
      <!-- Inherit the default GWT style sheet.  You can change       -->
      <!-- the theme of your GWT application by uncommenting          -->
      <!-- any one of the following lines.                            -->
      <inherits name='com.google.gwt.user.theme.standard.Standard'/>
      <!-- <inherits name="com.google.gwt.user.theme.chrome.Chrome"/> -->
      <!-- <inherits name="com.google.gwt.user.theme.dark.Dark"/>     -->

      <!-- Other module inherits                                      -->


      <!-- Specify the app entry point class.                         -->
      <entry-point class='com.google.gwt.sample.stockwatcher.client.StockWatcher'/>
    
      <!-- Specify the application specific style sheet.              -->
      <stylesheet src='StockWatcher.css' />
	
</module>
```

## Adding static HTML ##

The first method of styling your GWT application is to add HTML to your [host page](DevGuideHostPage.md).  Recall from our discussion of [user interface design](GettingStartedUserInterface.md) that one common way to use GWT is to attach one or more [RootPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/RootPanel.html)'s to placeholder elements in the host page's body (StockWatcher does this).  In addition to those placeholders, your host page can also contain any other HTML you want to include.

For our example, let's say that we want to add the Google Code logo to the top of the StockWatcher application:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/GoogleCode.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/GoogleCode.png)

To include static resources (like images) in our host page, we need to add them to our project's `public` directory (e.g., `src/com/google/gwt/sample/stockwatcher/public`).  The GWT compiler will automatically copy everything there to the output directory for deployment.  We can optionally create subdirectories in `public` to group resources together.  Let's go ahead and create an `images` directory at `src/com/google/gwt/sample/stockwatcher/public/images`.  Then copy the Google Code image from above and save it to the new directory.  You might need to refresh the `images` directory in your IDE to make the new file appear.

Now, modify the StockWatcher host page (`src/com/google/gwt/sample/stockwatcher/public/StockWatcher.html`) by adding an `<img>` tag pointing to the logo file:

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
    <img src="images/GoogleCode.png" />
    <h1>Stock Watcher</h1>
    <div id="stockList"></div>
  </body>
</html>
```

Refresh the hosted mode browser and you should see the Google Code logo at the top of the page (you may need to refresh hosted mode a couple of times when you add files to the `public` directory).

That was pretty easy, wasn't it?  StockWatcher is now branded with the Google Code logo.  However, the widgets themselves are still looking rather drab.

## Using CSS to style widgets ##

GWT widgets use [cascading style sheets](http://www.w3.org/Style/CSS/) (CSS) for visual styling.  Each widget is bound to one or more CSS rules which provide the style for that widget.

### Default styles ###

Each type of widget has a default style name derived from the widget class name.  For example, the [Button](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Button.html) widget has the style name `gwt-Button`, so by default all Button widgets inherit the style of the CSS class `gwt-Button`.  So, if you wanted all of your application's buttons to use a larger font, you could add the following rule to your CSS file (we'll explain how to add a CSS file to your project in just a bit):

```
$PP_OFF
.gwt-Button { font-size: 150%; }
```

You can override a widget's default style name by calling [setStyleName(Element, String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#setStyleName(com.google.gwt.user.client.Element,%20java.lang.String)).  This method will clear any existing styles (remember, a widget can be bound to more than one CSS class) and set the widget style to the CSS class you provide.

Using setStyleName(Element, String) allows us to do some simple widget styling, but it's not the most flexible approach.

### Secondary styles ###

GWT also has methods for adding and removing styles so you can easily bind one widget to multiple (changing) CSS classes.  For example, you might want to highlight your application's entry fields with a red border if the user enters invalid data.  In this case, you could create a CSS rule like the following:

```
.invalidEntry { border-color: red; } 
```

Now in your application code, you can easily flag any input widget as invalid by adding  `invalidEntry` to the widget's list of styles.  You would do this by calling [addStyleName(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#addStyleName(java.lang.String)) as follows:

```
someWidget.addStyleName("invalidEntry");
```

What's really handy is that when the user fixes the invalid input, you can just as easily remove the red border by calling [removeStyleName(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#removeStyleName(java.lang.String)):

```
someWidget.removeStyleName("invalidEntry");
```

When we add and remove style names using these methods, those styles are called _secondary styles_.  The reason is that they exist alongside the widget's _primary style_, which is the base style name for that widget.  By default, the primary style name of a widget will be the default style name for its widget class (e.g., `gwt-Button` for Button widgets).

The final appearance of a widget is determined by the sum of all the secondary styles added to it, plus its primary style.  You set the primary style of a widget with the [setStylePrimaryName(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#setStylePrimaryName(java.lang.String)) method.

To illustrate, let's say we have a [Label](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Label.html) widget.  In our CSS file, we have the following rules defined:

```
$PP_OFF
.MyText {
  color: blue;
}

.BigText {
  font-size: large;
}

.LoudText {
  font-weight:  bold;
}
```

Let's suppose we want a particular label widget to always display blue text, and in some cases, use a larger, bold font for added emphasis.  We could do something like this:

```
// set up our primary style
Label someText = new Label();
someText.setStylePrimaryName("MyText");
...

// later on, to really grab the user's attention
someText.addStyleName("BigText");
someText.addStyleName("LoudText");
...

// after the crisis is over
someText.removeStyleName("BigText");
someText.removeStyleName("LoudText");
```

Nothing too complicated going on here.  You may be wondering, though, why we need to distinguish between primary and secondary styles since they all seem to contribute to the widget's final style in the same way.  Well, it turns out there's a special type of secondary style called a _dependent style_.  Once you learn about dependent styles, young Padawan, you will understand.

### Dependent styles ###

Dependent style names are dependent upon the primary style name.  When you add a dependent style name to a widget, GWT will prefix the new style name with the widget's primary style name, separated with a dash ('-').  You can add or remove dependent styles with the [addStyleDependentName(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#addStyleDependentName(java.lang.String)) and [removeStyleDependentName(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#removeStyleDependentName(java.lang.String)) methods.

To see how this works we'll walk through a simple example.  Let's say you have a [TextBox](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TextBox.html) widget.  If we haven't called [setStylePrimaryName(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#setStylePrimaryName(java.lang.String)) to change it, the primary style name is `gwt-TextBox`.  Now suppose we want to show that the input in the text box is invalid, using the same indicator as before (red border).  If we use the following code:

```
someTextBox.addStyleDependentName("invalidEntry");
```

then both of the CSS style rules below will be applied:

```
$PP_OFF
.gwt-TextBox {
  font-size: 12pt;
}

.gwt-TextBox-invalidEntry {
  border-color: red; } 
}
```

When we called someTextBox.addStyleDependentName("invalidEntry"), GWT combined the primary style name `gwt-TextBox` with the dependent style name `invalidEntry` to create the final style name `gwt-TextBox-invalidEntry`.

Dependent styles are powerful because they are automatically updated whenever the primary style name changes.  Continuing with our example above, if you were to change the primary style name of your text box with this call:

```
someTextBox.setStylePrimaryName("myTexBox");
```

it would now get its styles from these two rules in your CSS file, replacing the original styles from before:

```
$PP_OFF
.myTextBox {
  font-size: 20pt;
}
 
.myTextBox-invalidEntry {
  background-color: red;
  color: white;
}
```

Secondary style names that are not dependent style names are _not_ automatically updated when the primary style name changes.

### Adding the CSS file ###

Now that you know all about the different kinds of styles and what they're good for, let's get back to StockWatcher.  Remember StockWatcher?  You know, that sample application we've been building all this time?  It's time to add some give it a new paint job with CSS styles.

To start, let's try to figure out what stylistic improvements we could make.

The first thing that jumps out at us is the tired-looking default font we're using.  We'll replace that with a nice sans-serif font.  Next we turn our attention to the watch list [FlexTable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/FlexTable.html).  Since we're displaying numerical data in the middle two columns, we should really be right-aligning those values.  In the last column, the Remove buttons should be centered and will need to be widened a bit.  Finally, let's color the table's header row and add some whitespace around the text box and Add button at the bottom.

Oh, and one more thing.  This one's actually an important usability enhancement.  Let's color the price change text for each stock to reflect the direction of the change.  Green for positive change, red for negative, and black for no change (or very small changes).  That way the user can tell at a glance the status of his watch list.

Here are the CSS rules to define each of the style changes we just described.

```
$PP_OFF
body {
  padding: 10px;
}

body, .watchList {
  font-family: arial, sans-serif;
  font-size: small;
}

.watchListHeader {
  background-color: #2062B8;
  color: white;
  font-style: italic;
}

.watchListNumericColumn {
  text-align: right;
}

.watchListRemoveColumn {
  text-align: center;
}

.gwt-Button-remove {
  width: 30px;
}

.positiveChange {
  color: Green;
}

.negativeChange {
  color: Red;
}

.noChange {
  color: Black;
}

.addPanel {
  margin: 25px 0px 25px 0px;
}
```

Like images, CSS files are static resources that are referenced from within our host page.  Your application already contains a style sheet called `StockWatcher.css`, so go ahead and save the CSS rules above into that file.

You can add additional style sheets in one of two ways.  First, you can add a link tag inside the `<head>` section of the host page `StockWatcher.html` by adding the following line (you also need to create a file called `MyStyleDefinitions.css` in the `public` directory):
```
<link type="text/css" rel="stylesheet" href="MyStyleDefinitions.css" />
```

Alternatively, you can add a style sheet in the `StockWatcher.gwt.xml` file by including a `stylesheet` tag.  If you open `StockWatcher.gwt.xml` now, you will see that `StockWatcher.css` is included in just this way.  To add your style sheet in the `gwt.xml` file, add the following line:
```
<stylesheet src='MyStyleDefinitions.css'/>
```

This is the standard way to load an external CSS file into an HTML document.  If this were a traditional web page, we would now go through the HTML source and add `class` attributes to various elements to give them the proper style.  However, that's not going to work with GWT widgets since they're created dynamically at runtime.  Instead, we'll use the methods we talked about earlier to add and change our widgets' styles.

### Settings styles from code ###

We'll start in onModuleLoad().  Here we'll style the watch list FlexTable and the panel containing the text box and Add button.  Notice that we can apply styles to an entire row of the FlexTable with one call using the [HTMLTable.RowFormatter](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLTable.RowFormatter.html) class.  There is also a corresponding [HTMLTable.ColumnFormatter](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLTable.ColumnFormatter.html) class.  However, we can't use it here because not all browsers support the `text-align` CSS property on table columns.  Instead, we use an [HTMLTable.CellFormatter](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLTable.CellFormatter.html) to add the styles directly to the header row cells

Also notice the call to [setCellPadding(int)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLTable.html#setCellPadding(int)).  This is to demonstrate that not all aspects of style are set via CSS.  As in the example, we can change a FlexTable's cell padding or cell spacing via its built-in methods.

```
public void onModuleLoad() {
  // set up stock list watch list
  stocksFlexTable.setText(0, 0, "Symbol");
  stocksFlexTable.setText(0, 1, "Price");
  stocksFlexTable.setText(0, 2, "Change");
  stocksFlexTable.setText(0, 3, "Remove");

  stocksFlexTable.setCellPadding(5);
  stocksFlexTable.addStyleName("watchList");
  stocksFlexTable.getRowFormatter().addStyleName(0, "watchListHeader");
  stocksFlexTable.getCellFormatter().addStyleName(0, 1, "watchListNumericColumn");
  stocksFlexTable.getCellFormatter().addStyleName(0, 2, "watchListNumericColumn");
  stocksFlexTable.getCellFormatter().addStyleName(0, 3, "watchListRemoveColumn");

  // set up event listeners for adding a new stock
  addButton.addClickListener(new ClickListener() {
    public void onClick(Widget sender) {
      addStock();
    }
  });
  
  newSymbolTextBox.addKeyboardListener(new KeyboardListenerAdapter() {
    public void onKeyDown(Widget sender, char keyCode, int modifiers) {
      if (keyCode == KEY_ENTER)
        addStock();
    }
  });
  
  // assemble Add Stock panel
  addPanel.add(newSymbolTextBox);
  addPanel.add(addButton);
  addPanel.addStyleName("addPanel");
```

The next stop is the addStock() function.  When we add a new row to the FlexTable, we again need to use an [HTMLTable.CellFormatter](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLTable.CellFormatter.html) to set the alignment properties for the last three columns.  For styling the Remove button, we add the dependent style name `remove`.  This means the button will get its style properties from CSS rules named `gwt-Button` (its primary style name) and `gwt-Button-remove` (its composite dependent style name).  In our `StockWatcher.css` file we only have a rule defined for the class `gwt-Button-remove`, so only those style properties will be applied).

The final styling change we need to implement is the changing color of the price change.  This is the one style that we'll change dynamically as StockWatcher runs.  One easy way to apply the new styles to the price change text without disturbing the alignment within the table cell (`text-align: right`) is to encapsulate the price change inside a nested Widget.  In our case, we'll use a simple [Label](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Label.html).

```
private void addStock() {
  final String symbol = newSymbolTextBox.getText().toUpperCase().trim();
  newSymbolTextBox.setFocus(true);
  
  // symbol must be between 1 and 10 chars that are numbers, letters, or dots
  if (!symbol.matches("^[0-9a-zA-Z\\.]{1,10}$"))
  {
    Window.alert("'" + symbol + "' is not a valid symbol.");
    newSymbolTextBox.selectAll();
    return;
  }
  
  newSymbolTextBox.setText("");
      
  // don't add the stock if it's already in the watch list
  if (stocks.contains(symbol))
    return;
  
  // add the stock to the list
  int row = stocksFlexTable.getRowCount();
  stocks.add(symbol);
  stocksFlexTable.setText(row, 0, symbol);
  stocksFlexTable.setWidget(row, 2, new Label());
  stocksFlexTable.getCellFormatter().addStyleName(row, 1, "watchListNumericColumn");
  stocksFlexTable.getCellFormatter().addStyleName(row, 2, "watchListNumericColumn");
  stocksFlexTable.getCellFormatter().addStyleName(row, 3, "watchListRemoveColumn");
  
  // add button to remove this stock from the list
  Button removeStock = new Button("x");
  removeStock.addStyleDependentName("remove");
  
  removeStock.addClickListener(new ClickListener(){
     public void onClick(Widget sender) {
       int removedIndex = stocks.indexOf(symbol);
       stocks.remove(removedIndex);
       stocksFlexTable.removeRow(removedIndex+1);
    }      
  });
  stocksFlexTable.setWidget(row, 3, removeStock);
  
  // get stock price
  refreshWatchList();
}
```

### Dynamically updating styles ###

To really add some flash to StockWatcher, we'll dynamically update the price change text to reflect the stock's movement.  For this we'll need to make some changes to the updateTable(StockPrice) function.

First, we'll need to get rid of this call to [setText(int,int,String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLTable.html#setText(int,%20int,%20java.lang.String)):

```
stocksFlexTable.setText(row, 2, changeText + " (" + changePercentText + "%)");
```

In our new version, every cell in column 2 contains a [Label](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Label.html) widget, so we'll have to get a reference to it and call _its_ [setText(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Label.html#setText(java.lang.String)) method instead.

The second change is to dynamically modify the Label's style name according the value returned from price.getChangePercent().  Note that when we update the style, we're using [setStyleName(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#setStyleName(java.lang.String)) instead of [addStyleName(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#addStyleName(java.lang.String)).  This way we remove any existing styles from the Label before applying the new one.  Here's the new `updateTable(StockPrice)` function:

```
private void updateTable(StockPrice price) {
  // make sure stock is still in our watch list
  if (!stocks.contains(price.getSymbol()))
    return;
  
  int row = stocks.indexOf(price.getSymbol()) + 1;

  // apply nice formatting to price and change
  String priceText = NumberFormat.getFormat("#,##0.00").format(price.getPrice());
  NumberFormat changeFormat = NumberFormat.getFormat("+#,##0.00;-#,##0.00");
  String changeText = changeFormat.format(price.getChange());
  String changePercentText = changeFormat.format(price.getChangePercent());

  // update the watch list with the new values
  stocksFlexTable.setText(row, 1, priceText);
  Label changeWidget = (Label)stocksFlexTable.getWidget(row, 2);
  changeWidget.setText(changeText + " (" + changePercentText + "%)");

  String changeStyleName = "noChange";
  if (price.getChangePercent() < -0.1f) {
    changeStyleName = "negativeChange";
  }
  else if (price.getChangePercent() > 0.1f) {
    changeStyleName = "positiveChange";
  }
  
  changeWidget.setStyleName(changeStyleName);
}
```

### New & Improved StockWatcher ###

With that, let's launch the restyled StockWatcher.  Remember what it looked like before:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDebug3.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherDebug3.png)

Here it is after our CSS transformation:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherStyled.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherStyled.png)

Not bad, eh?  A little good design goes a long way, with GWT applications.  As developers, we tend to be more interested in elegant algorithms and clever optimizations but remember that the user's opinion of our application will be formed _almost entirely_ on the interface's appearance and how well it works.  Don't neglect it!
