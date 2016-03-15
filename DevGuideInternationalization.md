# Internationalization #

GWT includes a flexible set of tools to help you internationalize your applications and libraries. GWT internationalization support provides a variety of techniques to internationalize strings, typed values, and classes.

## Quick Start with Internationalization ##

GWT supports a variety of ways of internationalizing your code. Start by researching which approach best matches your development requirements.

  * **Are you writing code from scratch?** <br>If so, you will probably want to read up on GWT's <a href='DevGuideStaticStringInternationalization.md'>static string internationalization</a> techniques.</li></ul>

<ul><li><b>Do you want to internationalize mostly settings or end-user messages?</b> <br>If you have mostly settings and interface labels with fixed text, consider <a href='http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html'>Constants</a>, which uses the least runtime resources. If you have a lot a of end-user messages where you want to add arguments to each message, then <a href='http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html'>Messages</a> is probably what you want.</li></ul>

  * **Do you have existing localized properties files you'd like to reuse?**  <br>The <a href='DevGuideI18nCreator.md'>i18nCreator tool</a> can automatically generate interfaces that extend either <a href='http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html'>Constants</a>, <a href='http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/ConstantsWithLookup.html'>ConstantsWithLookup</a> or <a href='http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html'>Messages</a>.</li></ul>

<ul><li><b>Are you adding GWT functionality to an existing web application that already has a localization process defined?</b> <br><a href='http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Dictionary.html'>Dictionary</a> will help you interoperate with existing pages without requiring you to use <a href='DevGuideSpecifyingLocale.md'>GWT's concept of locale</a>.</li></ul>

  * **Do you really just want a simple way to get properties files down to the client regardless of localization?**  <br>You can do that, too. Try using <a href='http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html'>Constants</a> without <a href='DevGuideSpecifyingLocale.md'>specifying a locale</a>.</li></ul>

<h2>Internationalization Techniques ##

GWT offers multiple internationalization techniques to afford maximum flexibility to GWT developers and to make it possible to design for efficiency, maintainability, flexibility, and interoperability in whichever combinations are most useful.

  * **[Static string internationalization](DevGuideStaticStringInternationalization.md)** <br>A family of efficient and type-safe techniques that rely on strongly-typed Java interfaces, <a href='DevGuidePropertiesFiles.md'>properties files</a>, and code generation to provide locale-aware messages and configuration settings. These techniques depend on the interfaces <a href='http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html'>Constants</a>,  <a href='http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/ConstantsWithLookup.html'>ConstantsWithLookup</a> and <a href='http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html'>Messages</a>.</li></ul>

<ul><li><b><a href='DevGuideDynamicStringInternationalization.md'>Dynamic string internationalization</a></b> <br>A simple and flexible technique for looking up localized values defined in a module's <a href='DevGuideHostPage.md'>host page</a> without needing to recompile your application. This technique is supported by the class <a href='http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Dictionary.html'>Dictionary</a>.</li></ul>

  * **[Extending or implementing Localizable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Localizable.html)** <br>Provides a method for internationalizing sets of algorithms using locale-sensitive type substitution. This is an advanced technique that you probably will not need to use directly, although it is useful for implementing complex internationalized libraries. For details on this technique, see the <a href='http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Localizable.html'>Localizable</a> class documentation.</li></ul>

<h2>The I18N Module ##

Core types related to internationalization:

  * [Constants](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html)  Useful for localizing typed constant values

  * [Messages](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html)  Useful for localizing messages requiring arguments

  * [ConstantsWithLookup](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/ConstantsWithLookup.html)  Like [Constants](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html) but with extra lookup flexibility for highly data-driven applications

  * [Dictionary](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Dictionary.html)  Useful when adding a GWT module to existing localized web pages

  * [Localizable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Localizable.html)  Useful for localizing algorithms encapsulated in a class

  * [DateTimeFormat](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/DateTimeFormat.html)  Formatting dates as strings.  See the section on [date and number formatting](DevGuideDateAndNumberFormat.md).

  * [NumberFormat](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/NumberFormat.html)  Formatting numbers as strings.  See the section on [date and number formatting](DevGuideDateAndNumberFormat.md).

The GWT internationalization types reside in the com.google.gwt.i18n package. To use any of these types, your module must inherit from the I18N module (com.google.gwt.i18n.I18N).

```
<module>
  <inherits name="com.google.gwt.i18n.I18N"/>
</module>
```

As of GWT 1.5, the User module (com.google.gwt.user.User) inherits the I18N module. So if your project's module XML file inherits the User module (which generally it does), it does not need to specify explicitly an inherit for the I18N module.

## Specifics ##
[Static String Internationalization](DevGuideStaticStringInternationalization.md)
A type-safe and optimized approach to internationalizing strings.

[Dynamic String Internationalization](DevGuideDynamicStringInternationalization.md)
A flexible and simplistic method of internationalizing strings that easily integrates with existing web applications that do not support the GWT `locale` client property.

[Specifying a Locale](DevGuideSpecifyingLocale.md)
How to add locales and specify the `locale` client property during deployment.

[Localized Properties Files](DevGuidePropertiesFiles.md)
How to create localized properties files for use with [Constants](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html) or [Messages](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html)

[Annotations for use with Internationalization](DevGuideI18NAnnotations.md)
New to GWT 1.5 is the use of annotations to allow more sophisticated internationalization. support, such as plurals and descriptions.
