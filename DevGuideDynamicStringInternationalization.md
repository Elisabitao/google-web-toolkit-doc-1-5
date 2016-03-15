# Dynamic String Internationalization #

For existing applications that may not support the GWT `locale` client property, GWT offers Dynamic String Internationalization to easily integrate GWT internationalization.

The [Dictionary](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Dictionary.html) class lets your GWT application consume strings supplied by the [host HTML page](DevGuideHostPage.md). This approach is convenient if your existing web server has a localization system that you do not wish to integrate with the [static string internationalization](DevGuideStaticStringInternationalization.md) methods. Instead, simply print your strings within the body of your HTML page as a JavaScript structure, and your GWT application can reference and display them to end users.
Since it binds directly to the key/value pairs in the host HTML, whatever they may be, the [Dictionary](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Dictionary.html) class is not sensitive to the [GWT locale setting](DevGuideSpecifyingLocale.md). Thus, the burden of generating localized strings is on your web server.

Dynamic string localization allows you to look up localized strings defined in a [host HTML](DevGuideHostPage.md) page at runtime using string-based keys.
This approach is typically slower and larger than the static string approach, but does not require application code to be recompiled when messages are altered or the set of locales changes.

> _Tip: The `Dictionary` class is completely dynamic, so it provides no static type checking, and invalid keys cannot be checked by the compiler. This is another reason we recommend using [static string internationalization](DevGuideStaticStringInternationalization.md) where possible._
