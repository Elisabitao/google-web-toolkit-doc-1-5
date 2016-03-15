# Localized Properties Files #

[Static string internationalization](DevGuideStaticStringInternationalization.md) uses traditional Java `.properties` files to manage translating tags into localized values.  These files may be placed into the same package as your main module class. They must be placed in the same package as their corresponding `Constants`/`Messages` subinterface definition file.

> _Tip: Use the [i18nCreator](DevGuideI18nCreator.md) script to get started._

```
$PP_OFF
 $ i18nCreator -eclipse Foo com.example.foo.client.FooConstants
 Created file src/com/example/foo/client/FooConstants.properties
 Created file FooConstants-i18n.launch
 Created file FooConstants-i18n
```


Both [Constants](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html) and [Messages](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html) use traditional Java properties files, with one notable difference: properties files used with GWT should be encoded as UTF-8 and may contain Unicode characters directly, avoiding the need for `native2ascii`. See the API documentation for the above interfaces for examples and formatting details.
Many thanks to the [Tapestry](http://tapestry.apache.org/) project for solving the problem of reading UTF-8 properties files in Tapestry's `LocalizedProperties` class.

In order to use internationalized characters, make sure that your host HTML file contains the `charset=utf8` content type in the meta tag in the header:

```
<meta http-equiv="content-type" content="text/html;charset=utf-8" />
```

You must also insure that all relevant source and `.properties` files are set to be the UTF-8 charset in your IDE.



### See Also ###

  * [Constants](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html)

  * [Messages](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html)
