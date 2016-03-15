# i18nCreator #

A command-line tool that generates [internationalization](DevGuideInternationalization.md) scripts for [static internationalization](DevGuideStaticStringInternationalization.md), along with sample [properties files](DevGuidePropertiesFiles.md).  Modify the `.properties` files to suit your project.  Run the `<classname>-i18n` script to generate a Java interface for accessing the tags defined in your properties files.


```
$PP_OFF
i18nCreator [-eclipse projectName] [-out dir] [-overwrite] [-ignore] [-createMessages] [-createConstantsWithLookup] interfaceName
```


|   `-eclipse`  |  Creates a debug launch config for the named Eclipse project |
|:--------------|:-------------------------------------------------------------|
|   `-out`      |  The directory to write output files into (defaults to current) |
|   `-overwrite`  |  Overwrite any existing files                                |
|   `-ignore`   |  Ignore any existing files; do not overwrite                 |
|   `-createMessages`  |  Generate scripts for a Messages interface rather than Constants |
|   `-createConstantsWithLookup`  |  Generate scripts for a ConstantsWithLookup interface rather than Constants |
|   `interfaceName`  |  The fully-qualified name of the interface to create         |

## Example ##

```
$PP_OFF
 ~/Foo> i18nCreator -eclipse Foo -createMessages com.example.foo.client.FooMessages
 Created file src/com/example/foo/client/FooMessages.properties
 Created file FooMessages-i18n.launch
 Created file FooMessages-i18n
```

Running `FooMessages-i18n` will generate an interface class in a file named `FooMessages.java` from `FooMessages.properties` that extends `Messages` (The messages will take parameters, substituting `{n}` with the nth parameter).

```
$PP_OFF
 ~/Foo> i18nCreator -eclipse Foo com.example.foo.client.FooConstants
 Created file src/com/example/foo/client/FooConstants.properties
 Created file FooConstants-i18n.launch
 Created file FooConstants-i18n
```

Running `FooConstants-i18n` will generate an interface from `FooConstants.properties` that extends `Constants` (The constants will not take parameters). To create a `ConstantsWithLookup` class, pass the `-createConstantsWithLookup` option.

In both examples, The launch configurations do the same thing as the scripts, but are intended to be run from within the Eclipse IDE.

> _Tip: When new entries are added to the properties file, the `-i18n` scripts must be run again.
>_

### See Also ###

  * [Static String Internationalization](DevGuideStaticStringInternationalization.md)