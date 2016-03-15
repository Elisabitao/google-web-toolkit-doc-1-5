# Internationalize #

Imagine that we've deployed StockWatcher, and it's a runaway success.  Online investors and traders love it... _if they happen to speak English_.  Because our current version only supports one language, we're missing out on a huge (and growing) international market.  Let's fix that.  We'll start by translating StockWatcher to German.  Why German, you ask?  Well, _warum nicht?_

## The I18N Module ##

GWT contains a number of classes dedicated to internationalization.  They're located in a separate GWT [module](DevGuideModules.md) named `com.google.gwt.i18n.I18N`.  So the first step in internationalizing StockWatcher is to add a reference to the `I18N` module from within our [module XML](DevGuideModuleXml.md), `StockWatcher.gwt.xml`.  All we need to do is add an `<inherits>` tag pointing to the internationalization module:

```
<inherits name="com.google.gwt.i18n.I18N"/>
```

## Internationalization techniques ##

There a few ways you can go about internationalizing a GWT application.  The simplest method is called _static string internationalization_.

[Static string internationalization](DevGuideStaticStringInternationalization.md) uses strongly-typed Java interfaces and standard `.properties` files to store translated strings.  This is the simplest option for internationalizing an application, and is the one we will be using to create a German version of StockWatcher.  Static string initialization requires very little overhead at runtime and is therefore a very efficient technique for translating both constant and parameterized strings.

Another method is [dynamic string internationalization](DevGuideDynamicStringInternationalization.md).  This method is slower than static string internationalization, but is very flexible.  Applications using this technique look up localized strings in the module's [host page](DevGuideHostPage.md) and do not need to be recompiled when you add a new locale.  This is a good option if you need to integrate a GWT application with an existing server-side localization system.

The final (and most powerful) method is implementing the  [Localizable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Localizable.html) interface.  Implementing Localizable allows you to go beyond simple string substitution and create localized versions of custom types.  It's an advanced internationalization technique that you probably won't have to use very often.

## Internationalizing StockWatcher ##

For StockWatcher, static string internationalization looks like the best choice.  The first thing we need to do is create the `.properties` files to hold the localized strings.  We'll then create Java classes to reference these strings by name.  Our new classes will need to implement one of three tag interfaces: [Constants](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html), [ConstantsWithLookup](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/ConstantsWithLookup.html), or [Messages](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html).

### Constants ###

Use the [Constants](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html) interface when you just need to store regular strings and can reference them statically at compile time.  Most of the strings in StockWatcher fit this description.  As we look at the user interface, we can easily find the constant strings that need to be translated:

  * The "Stock Watcher" title at the top
  * The four watch list column headings: "Symbol", "Price", "Change", and "Remove"
  * The "Add" on the button for adding new stocks


#### .properties files ####

> _**I have not reordered this section -- I think it works better to give the
> Java code first and then the translations.**_

Let's create a `.properties` files for these strings.  Add a new file to the `client` subpackage in the StockWatcher project named `StockWatcherConstants.properties` (in Eclipse, _File -> New -> File_).  Before you add any text, we'll need to change the encoding of the file to UTF-8.  That's because unlike standard Java `.properties` files, GWT `.properties` files may contain Unicode characters directly and should therefore be encoded as UTF-8.  In Eclipse, you can change the encoding of a file by right-clicking the file and selecting _Properties_ from the context menu:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherPropertiesFileEncoding.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherPropertiesFileEncoding.png)

Once you've changed the encoding of `StockWatcherconstants_de.properties`, add the following text and save the file:

We need to add their German translations to a new `.properties` file named `StockWatcherConstants_de.properties`.  As before, be sure to change the encoding to UTF-8 immediately after you create the file and _before_ you try to copy and paste the text below into the file:

`StockWatcherConstants_de.properties`

```
stockWatcher = Aktienbeobachter
symbol = Symbol
price = Kurs
change = Änderung
remove = Entfernen
add = Hinzufügen
```

Because our German `.properties` file contains a couple of international characters, we should also use UTF-8 encoding on our HTML host page.  Add the following tag to the host page's `<head>` tag:

```
<meta http-equiv="content-type" content="text/html;charset=utf-8" />
```

If you've never dealt with internationalization before, you may be wondering why we added the `_de` suffix to the end of our German `.properties` file.  `de` is the standard [language tag](http://www.w3.org/International/articles/language-tags/) for the German language.  Languages tags are abbreviations that indicate a document or application's locale.  In addition to specifying the language, they can also contain a subtag indicating the region of a locale.  For example, the language tag for French-speaking Canada is `fr_CA`.  In GWT, all `.properties` files contain a language code suffix indicating the file's locale (just like Java [resource bundles](http://java.sun.com/j2se/1.5.0/docs/api/java/util/ResourceBundle.html)).  The exception is the _default_ locale's `.properties` file.  The default is used when no locale is explicitly set at runtime.  StockWatcher's default locale is English (`en`).

The Developer Guide has more information about GWT [.properties files](DevGuidePropertiesFiles.md).

#### Constants interface ####

Now that we have our `.properties` files in place, we need to create the Java class to access them.  This class will implement the [Constants](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html) interface and will contain methods for each of the constants in our `.properties` files.  Create a new interface in StockWatcher's `client` subpackage named `StockWatcherConstants` with the following definition:

```
package com.google.gwt.sample.stockwatcher.client;

import com.google.gwt.i18n.client.Constants;

public interface StockWatcherConstants extends Constants {
  @DefaultStringValue("StockWatcher")
  String stockWatcher();

  @DefaultStringValue("Symbol")
  String symbol();

  @DefaultStringValue("Price")
  String price();

  @DefaultStringValue("Change")
  String change();

  @DefaultStringValue("Remove")
  String remove();

  @DefaultStringValue("Add")
  String add();
}
```

This interface will be bound to the `StockWatcherConstants*.properties` files automatically because of its name.  When we call one of StockWatcherConstants' methods at runtime, the `String` that is returned will be the corresponding string from the `.properties` file that represents the selected locale (we'll talk about setting the locale at runtime later on).  For example, when we call the stockWatcher() method, it will return `Stock Watcher` if the locale is set to English, or `Aktie Watcher` if it's set to German.

There is another interface, [ConstantsWithLookup](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/ConstantsWithLookup.html) which is like [Constants](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html) except that it also contains methods for looking up a localized string dynamically by name at runtime.

One more note: GWT includes the [i18nCreator](DevGuideI18nCreator.md) utility which can automate the creation of Java interfaces to access strings in `.properties` files.  You should check it out if you plan to internationalize a real GWT application and you already have your translatable strings in a properties file.

### Messages interface ###

> _**I have not reordered this section either -- I think it works better to give the
> Java code first and then the translations.**_

Many of the strings used in GWT applications may be parameterized _messages_.  Messages are strings with placeholders that are filled with values at runtime.  Think of the familiar [PrintStream.format(String, Object...)](http://java.sun.com/j2se/1.5.0/docs/api/java/io/PrintStream.html#format(java.lang.String,%20java.lang.Object...)) Java method for writing formatted strings.

To create localized messages, we'll need to implement the [Messages](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html) interface.  We'll also need to create a set of `.properties` files for translations just like last time.  Using the same procedure that we used before for constant strings, create the Java class first and then add the following `.properties` file to the `client` subpackage:

`StockWatcherMessages_de.properties`:

```
lastUpdate = Letzte Aktualisierung: {0}
invalidSymbol = ''{0}'' ist kein gültiges Aktiensymbol.
errorMsg = Fehler: {0}
delistedError = Aktiensymbol ''{0}'' ist nicht mehr gelistet
```

You'll notice that our message strings all have an `{0}` embedded within them.  These are placeholders that will be replaced at runtime by arguments passed to our StockWatcherMessages interface methods.  If you have a string that needs more than one argument, just number the placeholders sequentially (e.g., `myString = first is {0}, second is {1}, third is {2}, and so on`).

Also note that if your messages contains single quotes ('), as many of those in StockWatcher do, you'll need to replace them with two consecutive quotes in your `.properties` files.  In general, the formatting rules for GWT messages are the same that apply to Java's [MessageFormat](http://java.sun.com/j2se/1.5.0/docs/api/java/text/MessageFormat.html) class.

> _**Yes, two consecutive single quotes, not a double-quote character.**_

Now, we create the StockWatcherMessages interface we'll use to access the strings.  Unlike our `StockWatcherConstants` interface, the methods in this interface contain parameters.  When we call those methods, the arguments we pass will replace the placeholders we left in the strings in our `.properties` files.  Here is the contents of the new `StockWatcherMessages.java` file:

```
package com.google.gwt.sample.stockwatcher.client;

import com.google.gwt.i18n.client.Messages;

public interface StockWatcherMessages extends Messages {
  @DefaultMessage("Last update: {0}")
  String lastUpdate(String timestamp);

  @DefaultMessage("''{0}'' is not a valid symbol.")
  String invalidSymbol(String symbol);

  @DefaultMessage("Error: {0}")
  String errorMsg(String message);

  @DefaultMessage("Company ''{0}'' was delisted")
  String delistedError(String symbol);
}
```

> _**Not sure if we should introduce more advanced concepts here.  For example, instead
> of passing a separately-formatted string to lastUpdate, I would rather this be
> `"Last update: {0,date,medium} {0,time,medium}"` and passing a Date object to the method.
> Also, we could show numer formatting and currency patterns in the Messages interface,
> rather than formatting them separately, using
> `@DefaultMessage("Price is: {0,number,currency}") String price(double number)`
> etc.  We could also demonstrate plural forms, though that is less interesting for
> German since it has the same plural rules as English.**_

> _**On a related note, there are also annotations for interaction with the translation
> system (or translator).  For example, you can specify `@Meaning` to give the translator
> a hint as to what the phrase is supposed to mean (for instance is "orange" a color or
> a fruit?), provide examples for what a parameter may look like with
> `@Example("GOOG") String symbol`, etc -- not sure if we want to include that here
> or perhaps make a more advanced document describing how to use them.**_

### Replacing the strings ###

The next step in internationalizing StockWatcher is to replace all hard-coded strings within the source code to method calls to one of our two new interfaces.  However, there's one slight problem we need to resolve first.  One of our strings, the title of the application, is actually hard-coded into the HTML host page (`StockWatcher.html`), not in our Java source:

```
<h1>Stock Watcher</h1>
```

The easiest way to be able to set this string at runtime is to add a GWT [Label](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Label.html) widget and calls its [setText(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Label.html#setText(java.lang.String)) method.  This Label will need to reside within a [Panel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Panel.html).  You may remember from the Getting Started section on [user interface design](GettingStartedUserInterface.md) that we can wrap a [RootPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/RootPanel.html) around an HTML element in our host page by giving it an `id` attribute.  So replace the existing `<h1>` tag with this one:

```
<h1 id="appTitle"></h1>
```

Now, we should able to set all of our localized strings at runtime.  First, though, we'll need references to our StockWatcherConstants and StockWatcherMessages interfaces.  Add the following pair of instance fields to the `StockWatcher` class:

```
private StockWatcherConstants constants;
private StockWatcherMessages messages;
```

Since these are interfaces, not classes, we can't instantiate them directly.  Instead, we'll use the [GWT.create(Class)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class)) method.  Then we'll be able to use these interfaces' accessor methods to retrieve the appropriate strings.  Here's the updated onModuleLoad() with all hard-coded strings replaced with method calls:

```
public void onModuleLoad() {
  constants = GWT.create(StockWatcherConstants.class);
  messages = GWT.create(StockWatcherMessages.class);
  
  // set the window title, the header text, and the Add button text
  Window.setTitle(constants.stockWatcher());
  RootPanel.get("appTitle").add(new Label(constants.stockWatcher()));
  addButton = new Button(constants.add());

  // setup error message label
  errorMsgLabel.setStyleName("errorMessage");
  errorMsgLabel.setVisible(false);
    
  // set up stock list watch list
  stocksFlexTable.setCellPadding(5);
  stocksFlexTable.setText(0, 0, constants.symbol());
  stocksFlexTable.setText(0, 1, constants.price());
  stocksFlexTable.setText(0, 2, constants.change());
  stocksFlexTable.setText(0, 3, constants.remove());
  ...
}  
```

We can do the same to the other functions in the StockWatcher class:

```
private void addStock() {
  final String symbol = newSymbolTextBox.getText().toUpperCase().trim();
  newSymbolTextBox.setFocus(true);
  
  // symbol must be between 1 and 10 chars that are numbers, letters, or dots
  if (!symbol.matches("^[0-9a-zA-Z\\.]{1,10}$"))
  {
    Window.alert(messages.invalidSymbol(symbol));
    newSymbolTextBox.selectAll();
    return;
  }
  ...
  
}
```

```
private void refreshWatchList() {
...
 
  public void onFailure(Throwable caught) {
    // display the error message above the watch list
    String details = caught.getMessage();
    if (caught instanceof DelistedException) {
      details = messages.delistedError(((DelistedException)caught).getSymbol());
    }
    
    errorMsgLabel.setText(messages.errorMsg(details));
    errorMsgLabel.setVisible(true);
  }

...
}
```

```
private void updateTable(StockPrice[] prices) {
  ...
  
  // change the last update timestamp
  String timestamp = DateTimeFormat.getMediumDateTimeFormat().format(new Date());
  lastUpdatedLabel.setText(messages.lastUpdate(timestamp));
  ...
}
```

> _**Yes, the exclamation mark is a Wiki-formatting error (it is needed to indicate the
> following word should not be linked to another document, but inside a code section
> that isn't needed).**_

### Adding locale choices ###

The final step is to tell GWT that we now support the German (`de`) locale.  The way we do this is by extending the set of values of the `locale` _client property_.

Client properties in GWT are used to produce customized JavaScript _compilations_ of your GWT application, using a mechanism called [deferred binding](DevGuideDeferredBinding.md).  You should already know that the GWT compiler automatically generates different versions of your application, each one targeting a different web browser.  At runtime, the GWT bootstrap code delivers the appropriate one to the end user depending on which browser he is using.  These browser-specific compilations are created because _user agent_ is a GWT client property.  In the same way, GWT represents the locale as a client property.  This means that the GWT compiler will generate custom versions of internationalized applications representing each supported locale.  When there are multiple client properties GWT will generate a unique compilation for _every combination_ of possible client property values.  So, for example, if GWT supports 5 web browsers and you translate an application to 4 different languages, the GWT compiler will produce a total of 20 different versions of your application.  Each new user of your application will see the particular version matching his combination of web browser and locale.

To tell GWT which client properties our application uses and what values each property may be set to, we add `<extend-property>` tags to our application's [module XML](DevGuideModuleXml.md) file.  Since we just added one new value for the `locale` client property, we need to add the following to the `StockWatcher.gwt.xml` module file:

```
<extend-property name="locale" values="de"/>
```

If we translated StockWatcher to other languages as well, we would need to add an additional `<extend-property>` tag for each new locale:

```
<!-- French language, independent of country -->
<extend-property name="locale" values="fr"/>

<!-- French in Canada -->
<extend-property name="locale" values="fr_CA"/>

<!-- Russian language, independent of country -->
<extend-property name="locale" values="ru"/>

<!-- Portuguese language, independent of country -->
<extend-property name="locale" values="pt"/>
```

With the new `de` locale added to our StockWatcher module, let's try running in [web mode](DevGuideWebMode.md) again.  After you compile the internationalized StockWatcher, take a look at the output directory:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherI18nFiles.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherI18nFiles.png)

If you compare that to the set of files generated [before](GettingStartedWebMode.md) we added the new locale, you'll notice that there are now twice as many compilations as before.  The confirms what we learned about client properties in the discussion above.

## Setting the locale ##

Now that we have internationalized StockWatcher, how does GWT know which locale to load at runtime?  The answer is that it uses the value of the `locale` client property.  There are two ways to go about setting a GWT client property.

The first is to add a `<meta>` tag to the `<head>` of your HTML host page, containing the property name/value:

```
<meta name="gwt:property" content="locale=de">
```

The second option is to add the client property value to the query string of the URL:

```
http://www.example.org/myapp.html?locale=de
```

If you specify a client property (such as `locale`) in both the URL and in a `<meta>` tag, the URL value takes precedence.

> _**No, the meta tag goes into the host HTML page (in the head section), and specifies
> the default value of properties which aren't overridden by the property provider.  In
> the case of locale, the default property provider only looks at the URL parameter
> locale= besides the meta tag.**_

### Preserving the locale ###

The locale settings for a GWT module apply _only_ for that particular instance of that particular module.  This means that if your application contains links to other GWT host pages or non-GWT web pages on your site, the locale setting does not carry over to those pages.  Thus, if you want to preserve the user-specified locale, you'll either need to pass the locale in the query string of all links in your GWT application, or you'll need to store the locale setting somewhere on the server, which can then insert the appropriate `<meta>` tag into the host pages of any loaded GWT modules.

Now then, how do you know what to set the locale _to_ in the first place?  You might do as many websites do, and simply present the user with a list of languages/locales to mananually choose from.  You could also have the web server [check the Accept-Language](http://www.w3.org/International/questions/qa-accept-lang-locales) field in the browser's HTTP request to determine the correct locale.  If you do this, however, be sure to provide a way for the user to override the setting in case the browser's locale preference is not correct.

> _**Yes, "how do you know what to set the locale to in the first place" is referring to
> runtime determination of the previously-compiled locale to use.**_

> _**The discussion about browser preferences being incorrect is that many users do not
> actually configure their language preferences in the browser, so you can't just assume
> the value from Accept-Language is correct and not allow the user to change it.**_

## StockWatcher auf Deutsch ##

Ready to see our internationalized application in action?  Run StockWatcher and after it appears (in English, initially), add `?locale=de` to the end of the URL and hit Enter.  The full address should be:

```
http://localhost:8888/com.google.gwt.sample.stockwatcher.StockWatcher/StockWatcher.html?locale=de
```

When the application reloads it should have a distinctly German flavor:

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherGerman.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherGerman.png)

Notice that besides the translation of the user interface elements, setting the locale to German also affects the display format of the stock prices and the last update timestamp.  This all happens automatically thanks to our use of the [NumberFormat](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/NumberFormat.html) and [DateTimeFormat](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/DateTimeFormat.html) classes provided by GWT.

## More about internationalization ##

The Developer Guide contains more details about [internationalizing GWT applications](DevGuideInternationalization.md).  You may also want to check out the [W3C webpage](http://www.w3.org/International/) on internationalization for additional guidance.