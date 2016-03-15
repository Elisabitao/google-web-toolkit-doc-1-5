# Automatic Resource Inclusion #

Modules can contain references to external JavaScript and CSS files, causing them to be automatically loaded when the module itself is loaded.  This can be handy if your module is intended to be used as a reusable component because your module will not have to rely on the HTML host page to specify an external JavaScript file or stylesheet.

## Including External JavaScript ##

Script inclusion is a convenient way to automatically associate external JavaScript files with your module. Use the following syntax to cause an external JavaScript file to be loaded into the [host page](DevGuideHostPage.md) before your module entry point is called.

```
<script src="_js-url_"/>
```

The script is loaded into the namespace of the [host page](DevGuideHostPage.md) as if you had included it explicitly using the HTML `<script>` element. The script will be loaded before your [onModuleLoad()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/EntryPoint.html#onModuleLoad()) is called.

> _Versions of GWT prior to 1.4 required a script-ready function to determine when an included script was loaded. This is no longer required; all included scripts will be loaded when your application starts, in the order in which they are declared._

## Including External Stylesheets ##

Stylesheet inclusion is a convenient way to automatically associate external CSS files with your module. Use the following syntax to cause a CSS file to be automatically attached to the [host page](DevGuideHostPage.md).

```
<stylesheet src="_css-url_"/>
```

You can add any number of stylesheets this way, and the order of inclusion into the page reflects the order in which the elements appear in your module XML.

## Inclusion and Module Inheritance ##

Module inheritance makes resource inclusion particularly convenient. If you wish to create a reusable library that relies upon particular stylesheets or JavaScript files, you can be sure that clients of your library have everything they need automatically by inheriting from your module.

### See Also ###

  * [Module XML Format](DevGuideModuleXml.md)
