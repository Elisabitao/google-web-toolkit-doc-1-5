# Static String Internationalization #

Static String Internationalization is the most efficient way to localize your application for different locales in terms of runtime performance.   This approach is called "static" because it refers to creating tags that are matched up with human readable strings at compile time.  At compile time, mappings between tags and strings are created for all languages defined in the module.  The module startup sequence maps the appropriate implementation based on the locale setting using [deferred binding](DevGuideDeferredBinding.md).

Static string localization relies on code generation from standard Java [properties files](DevGuidePropertiesFiles.md).  GWT supports static string localization through two tag interfaces (that is, interfaces having no methods that represent a functionality contract) and a code generation library to generate implementations of those interfaces.

## Extending the Constants Interface ##

This example will walk through the process of creating a class of internationalized constant strings "hello, world" and "goodbye, world" in your GWT application.   The example will create a Java interface named `MyConstants` that abstracts those strings.  You can reference the `MyConstants` methods in your GWT code when you want to display one of those strings to the user and they will be translated appropriately for all locales that have matching `.properties` files.

Begin by creating a default properties file called `MyConstants.properties` in your GWT project.  You can place the file anywhere in your module's source path, but the `.properties` file and corresponding interface must be in the same package.  It's fine to place the file in the same package as your module's entry point class:

```
$PP_OFF
helloWorld = hello, world
goodbyeWorld = goodbye, world
```

You can also create a localized translation for each supported locale in separate properties files. The properties file must be named the same as our interface name, in our case `MyConstants`, with the appropriate suffix that indicates the [locale setting](DevGuideSpecifyingLocale.md). In this case, we localize for spanish using the filename `MyConstants_es.properties`:

```
$PP_OFF
helloWorld = hola, mundo
goodbyeWorld = adiós, mundo
```

Now define an interface that abstracts those strings by extending the built-in `Constants` interface.  Create a new Java interface in the same package where the `.properties` files were created. The method names must match the tag names uses in the `.properties` files:

```
public interface MyConstants extends Constants {
  String helloWorld();
  String goodbyeWorld();
}
```

> _Tip: The [i18nCreator](DevGuideI18nCreator.md) tool automates the generation of Constants interface subinterfaces like the one above.  The tool generates Java code so that you only need to maintain the `.properties` files.  It also works for `ConstantsWithLookup` and `Messages` classes.
>_


Note that `MyConstants` is declared as an interface, so you can not instantiate it directly with `new`.  To use the internationalized constants, you create a Java instance of `MyConstants` using the [GWT.create(Class)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class)) facility:

```
public void useMyConstants() {
  MyConstants myConstants = (MyConstants) GWT.create(MyConstants.class);
  Window.alert(myConstants.helloWorld());
}
```

> _Tip: In the GWT 1.5 release, `GWT.create()` returns a parameterized object so that the cast of the return value is no longer needed.
>_


You don't need to worry about the Java implementation of your static string classes.  Static string initialization uses a [deferred binding](DevGuideDeferredBinding.md) [generator](DevGuideDeferredBindingGenerators.md) which allows the GWT compiler to take care of automatically generating the Java code necessary to implement your `Constants` subinterface depending on the [locale](DevGuideSpecifyingLocale.md).

## Using the Messages Interface ##

The [Messages](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html) interface allows you to subsitute parameters into messages and to even re-order those parameters for different locales as needed.    The format of the messages in the properties files follows the specification in Java [MessageFormat](http://java.sun.com/j2se/1.5.0/docs/api/java/text/MessageFormat.html).  The interface you create will contain a java method with parameters matching those specified in the format string.

Here is an example Messages property value:

```
$PP_OFF
permissionDenied = Error {0}: User {1} Permission denied. 
```

The following code implements an alert dialog by substituting values into the message:

```
 public interface ErrorMessages extends Messages {
   String permissionDenied(int errorCode, String username);
 }
 ErrorMessages msgs = GWT.create(ErrorMessages.class)
  
 void permissionDenied(int errorVal, String loginId) {
   Window.alert(msgs.permissionDenied(errorVal, loginId));
 }
```


> _Note: Be advised that the rules for using quotes may be a bit confusing.  Refer to the [MessageFormat](http://java.sun.com/j2se/1.5.0/docs/api/java/text/MessageFormat.html) javadoc for more details._


## The Benefits of Static String Internationalization ##

As you can see from the example above, static internationalization relies on a very tight binding between internationalized code and its localized resources. Using explicit method calls in this way has a number of advantages. The GWT compiler can optimize deeply, removing uncalled methods and inlining localized strings -- making generated code as efficient as if the strings had been hard-coded.
The value of compile-time checking becomes even more apparent when applied to messages that take multiple arguments. Creating a Java method for each message allows the compiler to check both the number and types  of arguments supplied by the calling code against the message template defined in a properties file. For example, attempting to use the following interface and .properties files results in a compile-time error:

```
public interface ErrorMessages extends Messages {
  String permissionDenied(int errorCode, String username);
}
```

```
$PP_OFF
permissionDenied = Error {0}: User {1} does not have permission to access {2} 
```

An error is returned because the message template in the properties file expects three arguments, while the `permissionDenied` method can only supply two.

## Which Interface to Use? ##

Here are some guidelines to help choose the right interface for your application's needs:

  * Extend [Constants](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html) to create a collection of constant values of a variety of types that can be accessed by calling methods (called _constant accessors_) on an interface. Constant accessors may return a variety of types, including strings, numbers, booleans, and even maps. A compile-time check is done to ensure that the value in a properties file matches the return type declared by its corresponding constant accessor. In other words, if a constant accessor is declared to return an `int`, its associated property is guaranteed to be a valid `int` value -- avoiding a potential source of runtime errors.

  * The [ConstantsWithLookup](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/ConstantsWithLookup.html) interface is identical to `Constants` except that the interface also includes a method to look up strings by property name, which facilitates dynamic binding to constants by name at runtime. `ConstantsWithLookup` can sometimes be useful in highly data-driven applications. One caveat: `ConstantsWithLookup` is less efficient than `Constants` because the compiler cannot discard unused constant methods, resulting in larger applications.

  * Extend [Messages](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html) to create a collection of formatted messages that can accept parameters. You might think of the `Messages` interface as a statically verifiable equivalent of the traditional Java combination of `Properties`, `ResourceBundle`, and `MessageFormat` rolled into a single mechanism.


## Properties Files ##

All of the types above use properties files based on the traditional [Java properties file format](http://java.sun.com/j2se/1.4.2/docs/api/java/util/Properties.html#load(java.io.InputStream)), although GWT uses [an enhanced properties file format](DevGuidePropertiesFiles.md) that allows for UTF-8 and therefore allows properties files to contain Unicode characters directly.

### See Also ###

  * [Constants](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html)

  * [ConstantsWithLookup](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/ConstantsWithLookup.html)

  * [Messages](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html)

  * [Localized Properties Files](DevGuidePropertiesFiles.md)

  * [Dynamic String Internationalization](DevGuideDynamicStringInternationalization.md)

  * [Dictionary](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Dictionary.html)
