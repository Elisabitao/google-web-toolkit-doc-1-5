# I18N Improvements #

### Bi-directional support for standard widgets ###

The standard GWT widgets have been improved to support bidirectionality, as shown in the Showcase application. This represents significant support for right-to-left languages. See the Bi-di support section in [User interface improvements](ReleaseNotes_1_5_Ui.md) for more details.

### Support for plural forms, including rules for 99 languages ###

By specifying plural rules for your internationalized messages, and providing the plural form in respective locale properties files, the appropriate plural form will be chosen based on the message parameter's value. For example, let's say we wanted to apply to plural form to an English message. Using the new support for plural forms, we could define the following:

```
public interface MyMessages extends Messages {
  @DefaultMessage("You have {0} widgets.")
  @PluralText({"one", "You have 1 widget."})
  String widgetCount(@PluralCount int count);
}
```

Then, in your GWT application code, create the messages class as shown below:

```
MyMessages msg = GWT.create(MyMessages.class);
Window.alert(widgetCount(n));
```

The system will automatically choose the correct plural form based on the count. As long as you use the standard locale names for your property files, the correct pluralization rule will be picked up and applied.

### New LocaleInfo class lets you access information about the client locale ###

The new [LocaleInfo](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/LocaleInfo.html) class has been introduced in GWT 1.5 that allows you to discover more information about the client locale your application is running in. The [LocaleInfo](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/LocaleInfo.html) class can also return the list of locales available on the given client.

### Multiple Currency Support ###

[NumberFormat](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/NumberFormat.html) now supports multiple currencies via getCurrencyFormat(String currencyCode), and adjusts the number of decimal places for the currency.
