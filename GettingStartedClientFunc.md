# Implement client-side functionality #

Our StockWatcher sample has been coming along nicely so far.  To recap, we've designed and implemented the interface using GWT [widgets and panels](DevGuideWidgetGallery.md), and we've subscribed to [keyboard and click events](GettingStartedEvents.md) initiated by our users.  Now we're ready to write the client-side code to make our application _do something_.

## Validating user input ##

When the user first launches StockWatcher, he'll need to add stocks to his watch list by entering their ticker symbols (one at a time) into the textbox.  Before we add each stock, however, we should verify that the new symbol is valid.  If not, we'll warn him via a dialog box.

The first order of business is to add code to addStock() to extract the new stock symbol.  We'll use the [getText()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TextBoxBase.html#getText()) method of the [TextBox](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TextBox.html) widget to retrieve the text.  GWT widgets and panels contain a rich set of properties and methods, many of which correspond directly to members of the underlying HTML DOM elements.  Once we've converted the user input to a standard form we use a regular expression to check its format _(remember to use regexp's that have the same meaning in both Java and JavaScript)_.  Here's the updated addStock() method:

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
      
  // now we need to add the stock to the list...

}
```

Because of the call to [Window.alert(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/Window.html#alert(java.lang.String)), we'll also need to add another `import`:

```
import com.google.gwt.user.client.Window;
```

At this point, feel free to go ahead and recompile and launch the StockWatcher application to test the input validation.

_If you already have your hosted mode browser open, you don't need to restart it.  Just click the_Refresh_button on the toolbar to reload your updated GWT code._

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherInputValidation.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherInputValidation.png)

## Manipulating the watch list ##

Though StockWatcher has gained the ability to validate stock symbols, it still doesn't live up to its name: the user still can't add stocks to be watched.  Let's go ahead and work on that functionality now.  As you may recall, our interface contains a FlexTable widget named `stocksFlexTable` that will contain the watch list.  Each row in the list will contain the stock's symbol, its current price, its price change, and a button to remove the stock.  For now, let's just focus on adding and removing stocks, and not worry about setting the price and change values.

First, we'll need a data structure to hold the list of stock symbols we're currently watching.  Let's use the standard Java [ArrayList](http://java.sun.com/j2se/1.5.0/docs/api/java/util/ArrayList.html) and call our list `stocks`:

```
public class StockWatcher implements EntryPoint {

  private VerticalPanel mainPanel = new VerticalPanel();   
  private FlexTable stocksFlexTable = new FlexTable();
  private HorizontalPanel addPanel = new HorizontalPanel();
  private TextBox newSymbolTextBox = new TextBox();
  private Button addButton = new Button("Add");
  private Label lastUpdatedLabel = new Label();

  private ArrayList<String> stocks = new ArrayList<String>();
```

Don't forget the new `import`:

```
import java.util.ArrayList;
```

Next comes the fun part.  Add the following code to the end of the addStock() method:

```
// don't add the stock if it's already in the watch list
if (stocks.contains(symbol))
  return;

// add the stock to the list
int row = stocksFlexTable.getRowCount();
stocks.add(symbol);
stocksFlexTable.setText(row, 0, symbol);

// add button to remove this stock from the list
Button removeStock = new Button("x");
removeStock.addClickListener(new ClickListener(){
   public void onClick(Widget sender) {
     int removedIndex = stocks.indexOf(symbol);
     stocks.remove(removedIndex);
     stocksFlexTable.removeRow(removedIndex+1);
  }      
});
stocksFlexTable.setWidget(row, 3, removeStock);
```

The new addition should be relatively straightforward.  First, we're adding a new row to our FlexTable by settings the first column's text to the stock symbol (remember that its [setText(int, int, String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLTable.html#setText(int,%20int,%20java.lang.String)) method will automatically create new cells as needed, so we don't need to explicitly resize the table).  Next, we create a [Button](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Button.html) for removing the stock and place it in the last column of the table using the [setWidget(int, int, Widget)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLTable.html#setWidget(int,%20int,%20com.google.gwt.user.client.ui.Widget)) method (but not before giving it a [ClickListener](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/ClickListener.html) that removes the stock from the FlexTable and the ArrayList).

And now the moment of truth: launch (or refresh) the hosted mode browser.  Ta-da!  You should be able to add and remove stock symbols at will.  The only functionality we're missing now is the stocks' prices and change values.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherAddStocks.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherAddStocks.png)

## Refreshing the stock prices ##

The final and most important feature of StockWatcher is, shockingly enough, updating the prices of the stocks we're watching.  If we were writing the application using traditional web development techniques we would have rely on full page reloads every time we wanted to update the prices.  This could be accomplished either manually (the user clicking his browser's Refresh button) or automatically (for example, using a `<meta http-equiv="refresh" content="5">` tag in our HTML header).  But in this age of Web 2.0, that's simply not good enough.  Those wannabe day traders out there need their stock price updates _and they need them now_... without the annoying page refresh.

Fortunately for us GWT makes it easy to update our application's content on the fly.  Let's add automatic stock price updates to StockWatcher using the [Timer](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/Timer.html) class.

### Automatic refresh with Timer ###

Timer is a single-threaded, browser-safe timer class.  It allows us to schedule code to run at some point in the future, either once using [schedule(int)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/Timer.html#schedule(int)) or repeatedly with [scheduleRepeating(int)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/Timer.html#scheduleRepeating(int)).  We'll use the latter method to automatically update our stock prices every five seconds.

To use Timer, we create a new instance in our onModuleLoad() and override the [run()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/Timer.html#run()) method.  The run() method will be called when a timer fires.  In our case, we'll call a soon-to-be-written method named refreshWatchList() that will actually perform the update.  Add the import for [Timer](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/Timer.html) and then modify the end of the end of the onModuleLoad() method as follows:

```
import com.google.gwt.user.client.Timer;
```

```
public void onModuleLoad() {
  ...
  
  // add the main panel to the HTML element with the id "stockList"
  RootPanel.get("stockList").add(mainPanel);
  
  // setup timer to refresh list automatically
  Timer refreshTimer = new Timer() {
    public void run() {
      refreshWatchList();        
    }
  };
  refreshTimer.scheduleRepeating(REFRESH_INTERVAL);

  // move cursor focus to the text box
  newSymbolTextBox.setFocus(true);
}
```

In addition to the change to onModuleLoad(), you'll also need to define the constant that specifies the refresh rate.  Add that to the top of the `StockWatcher` class.

```
private static final int REFRESH_INTERVAL = 5000; // ms
```

Before we continue, we need to add one more call to the new refreshWatchList() method.  We'll call it in addStock() immediately after we add the new stock to the FlexTable (so the new stock gets its price & change values right away):

```
private void addStock() {  
  ...
  
  Button removeStock = new Button("x");
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

private void refreshWatchList() {
  // Code will be added in the next section
}
```

### StockPrice class ###

One of the primary ways GWT speeds AJAX development is by allowing us to write our applications in the Java language.  Because of this, we can take advantage of static type checking and time-tested patterns of object-oriented programming, which when combined with modern IDE features like code completion and automated refactoring, making it easier than ever to write robust AJAX applications with well organized code bases.

For StockWatcher, we'll take advantage of this capability by factoring a stock's price data into its own class.  Create a new Java class named `StockPrice` in the `com.google.gwt.sample.stockwatcher.client` package (in Eclipse, _File -> New -> Class_).

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/GettingStartedStockPriceClass.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/GettingStartedStockPriceClass.png)

Here's the complete implementation of our new class:

```
package com.google.gwt.sample.stockwatcher.client;

public class StockPrice {
  
  private String symbol;
  private double price;
  private double change;
  
  public StockPrice() {
  }
  
  public StockPrice(String symbol, double price, double change) {
    this.symbol = symbol;
    this.price = price;
    this.change = change;
  }
      
  public String getSymbol() {
    return this.symbol;
  }
  
  public double getPrice() {
    return this.price;
  }
  
  public double getChange() {
    return this.change;
  }
  
  public double getChangePercent() {
    return 10.0 * this.change / this.price;
  }
  
  public void setSymbol(String symbol) {
    this.symbol = symbol;
  }
  
  public void setPrice(double price) {
    this.price = price;
  }
  
  public void setChange(double change) {
    this.change = change;
  }
}
```

### Generating stock prices ###

Now that we have a StockPrice class to encapsulate stock price data, let's use it to update the watch list table.  First, we need to generate the actual data.  In lieu of actually retrieving real-time stock prices from an online data source, let's just use GWT's built in [Random](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/Random.html) class to create pseudo-random price and change values.  We'll populate an array of StockPrice objects with these values and then pass them on to another function to update the watch list FlexTable.  Add the following `import` and function to our StockWatcher class:

```
import com.google.gwt.user.client.Random;
```

```
private void refreshWatchList() {
  final double MAX_PRICE = 100.0; // $100.00
  final double MAX_PRICE_CHANGE = 0.02; // +/- 2%

  StockPrice[] prices = new StockPrice[stocks.size()];
  for (int i=0; i<stocks.size(); i++) {
    double price = Random.nextDouble() * MAX_PRICE;
    double change = price * MAX_PRICE_CHANGE * (Random.nextDouble() * 2.0 - 1.0);
      
    prices[i] = new StockPrice((String)stocks.get(i), price, change);
  }
  
  updateTable(prices);
}
```

### Updating the watch list ###

The last two new functions will update the display with the new prices.  In updateTable(StockPrice) you can see an example of using the [NumberFormat](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/NumberFormat.html) class to format numeric values into Strings.  In our case, we're using it to format the price as a currency (separator with two decimal places) and to display a sign indicator in front of the price change.  Similarly, notice the use of the [DateTimeFormat](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/DateTimeFormat.html) class in the updateTable(StockPrice[.md](.md)) method to format the current date and time.  Go ahead and add both of these functions to StockWatcher:

```
private void updateTable(StockPrice[] prices) {
  for (int i=0; i<prices.length; i++) {
    updateTable(prices[i]);
  }
  
  // change the last update timestamp
  lastUpdatedLabel.setText("Last update : " + 
      DateTimeFormat.getMediumDateTimeFormat().format(new Date()));
}

private void updateTable(StockPrice price) {
  // make sure the stock is still in our watch list
  if (!stocks.contains(price.getSymbol())) {
    return;
  }
  
  int row = stocks.indexOf(price.getSymbol()) + 1;
  
  // apply nice formatting to price and change
  String priceText = NumberFormat.getFormat("#,##0.00").format(price.getPrice());
  NumberFormat changeFormat = NumberFormat.getFormat("+#,##0.00;-#,##0.00");
  String changeText = changeFormat.format(price.getChange());
  String changePercentText = changeFormat.format(price.getChangePercent());

  // update the watch list with the new values
  stocksFlexTable.setText(row, 1, priceText);
  stocksFlexTable.setText(row, 2, changeText + " (" + changePercentText + "%)");
}
```

The [DateTimeFormat](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/DateTimeFormat.html) and [NumberFormat](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/NumberFormat.html) classes are in a separate package so you'll also need to add another `import` statement for them, as well as one for Java's standard [Date](http://java.sun.com/j2se/1.4.2/docs/api/java/util/Date.html) class:

```
import com.google.gwt.i18n.client.DateTimeFormat;
import com.google.gwt.i18n.client.NumberFormat;

import java.util.Date;
```

You may have noticed that DateTimeFormat and NumberFormat live in a subpackage of `com.google.gwt.i18`, which suggest that they deal with internationalization in some way.  And indeed they do... both classes will automatically use your application's locale setting when formatting numbers and dates.  We'll talk more about [localizing and translating your GWT application](GettingStartedI18n.md) later in the Getting Started guide.

Time to test our changes again.  Launch or refresh the application in hosted mode and try adding some stocks to the watch list.  Now, we can see each stock's current price and change.  If you watch the list for just a bit, you should also notice those values changing every five seconds, along with the timestamp at the bottom.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherPriceUpdates.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherPriceUpdates.png)

Well, it looks like our StockWatcher application is working perfectly.  Or is it?  As it turns out, there's one subtle bug that slipped in.  See if you can spot it (_hint: it's visible in the screenshot above_).  In the next section, we'll use our Java debugging skills to track it down.
