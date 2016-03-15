# Defining a module: format of module XML files #
Modules are defined in XML files with a file extension of _.gwt.xml_.
Module XML files should reside in your project's root package.

If you are using the [standard project structure](DevGuideDirectoriesPackageConventions.md), your module XML can be as simple as the following example:

```
 <module>
    <inherits name="com.google.gwt.user.User" />
    <entry-point class="com.example.cal.client.CalendarApp" />
 </module>
```

## Loading modules ##
Module XML files are found on the Java classpath.
Modules are always referred to by their logical names.
The logical name of a module is of the form _pkg1.pkg2.ModuleName_ (although any number of packages may be present).
The logical name includes neither the actual file system path nor the file extension.

For example, if the module XML file has a file name of...

```
$PP_OFF
~/src/com/example/cal/Calendar.gwt.xml
```

...then the logical name of the module is:

```
$PP_OFF
com.example.cal.Calendar
```

## Renaming modules ##
The `<module>` element supports an optional attribute `rename-to` that causes the compiler to behave as though the module had the name specified. Renaming a module has two primary use cases:
  * to have a shorter module name that doesn't reflect the actual package structure
  * to create a "working module" to speed up development time by restricting the number of permutations

`com.foo.WorkingModule.gwt.xml`:
```
<module rename-to="com.foo.MyModule">
  <inherits name="com.foo.MyModule" />
  <set-property name="user.agent" value="ie6" />
  <set-property name="locale" value="default" />
</module>
```

When `WorkingModule.gwt.xml` is compiled, the compiler will produce only an `ie6` variant using the default locale; this will speed up development compilations.  The output from the `WorkingModule.gwt.xml` will be a drop-in replacement for `MyModule.gwt.xml` because the compiler will generate the output using the alternate name.

To use a renamed module in hosted mode, it is necessary to use the "physical" name (i.e. `com.foo.WorkingModule`) in the URL path, while otherwise referring to `com.foo.MyModule` in the host HTML page.

## Dividing code into multiple modules ##
Creating a second module doesn't necessarily mean that that module must define an entry point. Typically, you create a new module when you want to package up a library of GWT code that you want to reuse in other GWT projects. An example of this is the Google API Library for Google Web Toolkit ([GALGWT](http://code.google.com/p/gwt-google-apis/)), specifically the Gears for GWT API binding. If you download the library and take a look at the `gwt-google-apis/com/google/gwt/gears` you'll find the `Gears.gwt.xml` file for the module which doesn't define an entry point. However, any GWT project that would like to use Gears for GWT will have to inherit the Gears.gwt.xml module. For example, a module named "Foo" might want to use GALGWT, so in `Foo.gwt.xml` an `<inherits>` entry would be needed:

```
<module>
...
    <inherits name='com.google.gwt.gears.Gears' />
```

## Loading multiple modules in an HTML host page ##
If you have multiple GWT modules in your application, there are two ways to approach loading them.

  1. Compile each module separately and include each module with a separate `<script>` tag in your [HTML host page](DevGuideHostPage.md).
  1. Create a top level module XML definition that includes all the modules you want to include.  Compile the top level module to create a single set of JavaScript output.

The first approach of compiling each module separately might seem like a compelling modular approach. Unfortunately, this approach is probably worst for performance because two full applications will need to be downloaded.  Furthermore, each module will contain redundant copies of GWT library code and possibly encounter conflicts when it comes to event handling.  For these reasons, you should not use this approach without a compelling reason.

## Controlling compiler output ##
The GWT compiler separates the act of compiling and packaging its output with the Linker subsystem.  It is responsible for the final packaging of the JavaScript code and providing a pluggable bootstrap mechanism for any particular deployment scenario.

  * `<`define-linker name="_short\_name_" class="_fully\_qualified\_class\_name_" /`>` : Register a new Linker instance with the compiler.  The `name` attribute must be a valid Java identifier and is used to identify the Linker in `<add-linker>` tags.  It is permissible to redefine an already-defined Linker by declaring a new `<define-linker>` tag with the same name.  Linkers are divided into three categories, PRE, POST, and PRIMARY.  Exactly one primary linker is run for a compilation.  Pre-linkers are run in lexical order before the primary linker, and post-linkers are run in reverse lexical order after the primary linker.
  * `<`add-linker name="_linker\_name_" /`>` : Specify a Linker to use when generating the output from the compiler.  The `name` property is a previously-defined Linker name.  This tag is additive for pre- and post-linkers; only the last primary linker will be run.

Several linkers are provided by `Core.gwt.xml`, which is automatically inherited by `User.gwt.xml`.
  * **std** : The standard iframe-based bootstrap deployment model.
  * **xs** : The cross-site deployment model.
  * **sso** : This Linker will produce a monolithic JavaScript file.  It may be used only when there is a single distinct compilation result.

From `Core.gwt.xml`:
```
<module>
   <define-linker name="std" class="com.google.gwt.dev.linker.IFrameLinker" />
   <define-linker name="sso" class="com.google.gwt.dev.linker.SingleScriptLinker" />
   <define-linker name="xs" class="com.google.gwt.dev.linker.XSLinker" />
   <add-linker name="std" />
</module>
```

Changing the desired linker in `MyModule.gwt.xml`:
```
<module>
  <inherits name="com.google.gwt.core.Core" />
  <add-linker name="xs" />
</module>
```

## Overriding one package implementation with another ##
The `<super-source>` tag instructs the compiler to "re-root" a source path.  This is useful for cases where you want to be re-use an existing Java API for a GWT project, but the original source is not available or not translatable.  A common reason for this is to emulate part of the JRE not implemented by GWT.

For example, suppose you want implement the UUID class provided by the JRE under `java.util`.  Assume your project's module file is `com/example/myproject/MyProject.gwt.xml`.   Place the source for the UUID class into `com/example/myproject/jre/java/util/UUID.java`.  Then add a line to `MyProject.gwt.xml`:

```
    <super-source path="jre" />
```

This tells the compiler to add all subfolders of `com/example/myproject/jre/` to the [source path](DevGuideModules.md), but to strip off the path prefix up to and including `jre`.  As a result, `com/google/myproject/gwt/jre/java/util/UUID.java` will be visible to the compiler as `java/util/UUID.java`, which is the intended result.

The GWT project uses this technique internally for the JRE emulation classes provided with GWT.  One caveat specific to overriding JRE classes in this way is that they will never actually be used in hosted mode.  In hosted mode, the native JRE classes always supersede classes compiled from source.

The `<super-source>` element supports [pattern-based filtering](DevGuidePublicPackageFiltering.md) to allow fine-grained control over which resources get copied into the output directory during a GWT compile.

# XML Element Reference #
This section documents the most commonly used elements in the module XML file.

  * `<`inherits name="_logical-module-name_" /`>` : Inherits all the settings from the specified module as if the contents of the inherited module's XML were copied verbatim. Any number of modules can be inherited in this manner. See the FAQ for [commonly used modules](FAQ_GWTModuleInheritance.md).

  * `<`entry-point class="_classname_" /`>` : Specifies an [entry point](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/EntryPoint.html) class. Any number of entry-point classes can be added, including those from inherited modules.  Entry points are all compiled into a single codebase.  They are called sequentially in the order in which they appear in the module file.  So when the `onModuleLoad()` of your first entry point finishes, the next entry point is called immediately.

  * `<`source path="_path_" /`>` : Each occurrence of the `<source>` tag adds a package to the [source path](DevGuideModules.md) by combining the package in which the module XML is found with the specified path to a subpackage. Any Java source file appearing in this subpackage or any of its subpackages is assumed to be translatable.  The `<source>` element supports [pattern-based filtering](DevGuidePublicPackageFiltering.md) to allow fine-grained control over which resources get copied into the output directory during a GWT compile.

> If no `<source>` element is defined in a module XML file, the _client_ subpackage is implicitly added to the source path as if `<source path="client" />` had been found in the XML. This default helps keep module XML compact for standard project layouts.

  * `<`public path="_path_" /`>` : Each occurrence of the `<public>` tag adds a package  to the [public path](DevGuideModules.md) by combining the package in which the module XML is found with the specified path to identify the root of a public path entry. Any file appearing in this package or any of its subpackages will be treated as a publicly-accessible resource. The `<public>` element supports [pattern-based filtering](DevGuidePublicPackageFiltering.md) to allow fine-grained control over which resources get copied into the output directory during a GWT compile.

> If no `<public>` element is defined in a module XML file, the _public_ subpackage is implicitly added to the public path as if `<public path="public">` had been found in the XML. This default helps keep module XML compact for standard project layouts.

  * `<`servlet path="_url-path_" class="_classname_" /`>` : For convenient RPC testing, this element loads a servlet class mounted at the specified URL path. The URL path should be absolute and have the form of a directory (for example, `/spellcheck`). Your client code then specifies this URL mapping in a call to [ServiceDefTarget.setServiceEntryPoint(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/ServiceDefTarget.html#setServiceEntryPoint(java.lang.String)). Any number of servlets may be loaded in this manner, including those from inherited modules.

> The `<servlet>` element applies only to GWT's embedded server server-side debugging feature.

  * `<`script src="_js-url_" /`>` : Automatically injects the external JavaScript file located at the location specified by _src_. See [automatic resource inclusion](DevGuideAutomaticResourceInjection.md) for details.

  * `<`stylesheet src="_css-url_" /`>` : Automatically injects the external CSS file located at the location specified by _src_. See [automatic resource inclusion](DevGuideAutomaticResourceInjection.md) for details.

  * `<`extend-property name="_client-property-name_" values="_comma-separated-values_" /`>` : Extends the set of values for an existing client property. Any number of values may be added in this manner, and client property values accumulate through inherited modules. You will likely only find this useful for [specifying locales in internationalization](DevGuideSpecifyingLocale.md).


## Elements for Deferred Binding ##
The following elements are used for defining [deferred binding](DevGuideDeferredBinding.md) rules.  Deferred binding is not commonly used in user projects.

  * `<`replace-with class="_replacement\_class\_name_"`>` : A directive to use deferred binding with replacement.

  * `<`generate-with class="_generator\_class\_name_"`>` : A directive to use deferred binding using a [Generator](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/ext/Generator.html)

  * `<`define-property name="_property\_name_" values="_property\_values_"`>` : Define a property and allowable values (comma-separated identifiers).  This element is typically used to generate a value that will be evaluated by a rule using a `<when...>` element.

  * `<`set-property name="_property\_name_" value="_property\_value_"`>` : Set the value of a previously-defined property (see `<define property>` above).   This element is typically used to generate a value that will be evaluated by a rule using a `<when...>` element. Note that `set-property` and `property-provider` on the same value will overwrite each other.  The last definition encountered is the one that is used.

  * `<`property-provider name="_property\_name_"`>` : Define a JavaScript fragment that will return the value for the named property at runtime.  This element is typically used to generate a value that will be evaluated in a `<when...>` element.  To see examples of  `<property-provider>` definitions in action, see the files `I18N.gwt.xml` and `UserAgent.gwt.xml` in the GWT source code.  Note that `set-property` and `property-provider` on the same value will overwrite each other.  The last definition encountered is the one that is used.

### Defining conditions ###
The `<replace-with-class>` and `<generate-with-class>` elements can take a `<when...>` child element that defines when this rule should be used, much like the `WHERE` predicate of an SQL query.  The three different types of predicates are:

  * `<`when-property-is name="_property\_name_" value="_value_" /`>` :  Deferred binding predicate that is true when a named property has a given value.

  * `<`when-type-assignable class="_class\_name_" /`>` : Deferred binding predicate that is true for types in the type system that are assignable to the specified type.

  * `<`when-type-is class="_class\_name_" /`>` : Deferred binding predicate that is true for exactly one type in the type system.

Several different predicates can be combined into an expression.  Surround your `<when...>` elements using the following nesting elements begin/end tags:

  * `<all>` _when\_expressions_ `</all>` : Predicate that ANDs all child conditions.

  * `<any>` _when\_expressions_ `</any>` : Predicate that ORs all child conditions.

  * `<none>` _when\_expressions_ `</none>` : Predicate that NANDs all child conditions.

### Deferred Binding Example ###
As an example module XML file that makes use of deferred binding rules, here is a module XML file from the  GWT source code, Focus.gwt.xml:

```
<module>
  <inherits name="com.google.gwt.core.Core" />
  <inherits name="com.google.gwt.user.UserAgent" />

  <!-- old Mozilla, and Opera need a different implementation -->
  <replace-with class="com.google.gwt.user.client.ui.impl.FocusImplOld">
    <when-type-is class="com.google.gwt.user.client.ui.impl.FocusImpl" />
      <any>
        <when-property-is name="user.agent" value="gecko" />
        <when-property-is name="user.agent" value="opera" />
      </any>
  </replace-with>

  <!--  Safari needs a different hidden input -->
  <replace-with class="com.google.gwt.user.client.ui.impl.FocusImplSafari">
    <when-type-is class="com.google.gwt.user.client.ui.impl.FocusImpl" />
    <when-property-is name="user.agent" value="safari" />
  </replace-with>

  <!-- IE's implementation traps exceptions on invalid setFocus() -->
  <replace-with class="com.google.gwt.user.client.ui.impl.FocusImplIE6">
  <when-type-is class="com.google.gwt.user.client.ui.impl.FocusImpl" />
    <any>
      <when-property-is name="user.agent" value="ie6" />
    </any>
  </replace-with>
</module>
```
