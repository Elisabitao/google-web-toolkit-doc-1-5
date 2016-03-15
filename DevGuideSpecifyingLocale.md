# Specifying a Locale #

GWT represents `locale` as a client property whose value can be set either using a meta tag embedded in the [host page](DevGuideHostPage.md) or in the query string of the host page's URL. Rather than being supplied by GWT, the set of possible values for the `locale` client property is entirely a function of your [module configuration](DevGuideModules.md).

## Client Properties and the GWT Compilation Process ##
_Client properties_ are key/value pairs that can be used to configure GWT modules. User agent, for example, is represented by a client property. Each client property can have any number of values, but all of the values must be enumerable when the GWT compiler runs.
GWT modules can define and extend the set of available client properties along with the potential values each property might assume when loaded in an end user's browser using the `<extend-property>` directive.  At compile time, the GWT compiler determines all the possible permutations of a module's client properties, from which it produces multiple _compilations_. Each compilation is optimized for a different set of client properties and is recorded into a file ending with the suffix `.cache.html`.


In deployment, the end-user's browser only needs one particular compilation, which is determined by mapping the end user's client properties onto the available compiled permutations. Thus, only the exact code required by the end user is downloaded, no more. By making locale a client property, the standard startup process in `<module>.nocache.js` chooses the appropriate localized version of an application, providing ease of use, optimized performance, and minimum script size.  See the [Knowledge Base](FAQ_GWTApplicationFiles.md) for more information about the logic of the `<modulename>.nocache.js` file.

## The Default Locale ##

The `com.google.gwt.i18n.I18N` module defines only one locale by default, called `default`. This default locale is used when the `locale` client property goes unspecified in deployment. The default locale is used internally as a last-resort match between a [Localizable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Localizable.html) interface and a localized resource or class.

## Adding Locale Choices to a Module ##
In any real-world application, you will define at least one locale in addition to the default locale. "Adding a locale" means extending the set of values of the `locale` client property using the `<extend-property>` element in your [module XML](DevGuideModuleXml.md).
For example, the following module adds multiple locale values:

```
<module>
  <inherits name="com.google.gwt.user.User"/>
  <inherits name="com.google.gwt.i18n.I18N"/>
  
  <!-- French language, independent of country -->
  <extend-property name="locale" values="fr"/>

  <!-- French in France -->
  <extend-property name="locale" values="fr_FR"/>

  <!-- French in Canada -->
  <extend-property name="locale" values="fr_CA"/>
  
  <!-- English language, independent of country -->
  <extend-property name="locale" values="en"/>
</module>
```

## Choosing a Locale at Runtime ##

The locale client property can be specified using either a meta tag or as part of the query string in the host page's URL. If both are specified, the query string takes precedence.
To specify the `locale` client property using a meta tag in the [host page](DevGuideHostPage.md), embed a meta tag for `gwt:property` as follows:

```
<meta name="gwt:property" content="locale=x_Y">
```

For example, the following host HTML page sets the locale to "ja\_JP":

```
<html>
  <head>
    <meta name="gwt:property" content="locale=ja_JP">
  </head>
  <body>
    <!-- Load the GWT compiled module code                           -->
    <script src="com.google.gwt.examples.i18n.ColorNameLookupExample.nocache.js " />
  </body>
</html>
```

To specify the `locale` client property using a query string, specify a value for the name `locale`. For example,

```
$PP_OFF
http://www.example.org/myapp.html?locale=fr_CA
```


> _Tip: The preferred time to explicitly set `locale` is to do so before your GWT module is invoked.  You can change the `locale` from within your GWT module by adding or changing the `locale` query string in the current URL and reloading the page.  Keep in mind that after reloading the page, your module will restart.
>_

## Creating a New Property Provider ##

If you are embedding your module into an existing application, there may be another way of determining locale that does not lend itself to using the `<meta>` tag or specifying `locale=` as a query string.  In this case, you could write your own property provider.

A property provider is specified in the [module XML file](DevGuideModuleXml.md) as a JavaScript fragment that will return the value for the named property at runtime.  In this case, you would want to define the locale property using a property provider.   To see examples of  `<property-provider>` definitions in action, see the files `I18N.gwt.xml` and `UserAgent.gwt.xml` in the GWT source code.
