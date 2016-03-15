# Miscellaneous Features & Improvements #

### The GWT class has two new convenience methods ###
  * `GWT.getVersion()`: reports what version of GWT was used to compile your application.
  * `GWT.isClient()`: can be used to detect whether shared code is running in the browser (as opposed to on a server).

### Call Java constructors from JSNI code ###
> Java constructors can now be called directly from JSNI via the ::new method reference.

### Access underlying JS object when catching a JavaScriptException ###
> The [JavaScriptException](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JavaScriptException.html) class has been improved to provide access to the underlying JS object when catching the exception.

### Improvements or changes in usage to JRE classes ###
  * Use `Duration.currentTimeMillis()` instead of `System.currentTimeMillis()`, since long is now emulated.
  * `Arrays.asList()` is a really efficient array-backed collection; if you already have an array sitting around, it allows you to avoid the linear-time copy of all the elements.

### Improvements to JUnit Testing and Benchmark components ###
  * Added GWTTestSuite classes to optimize the order in which GWTTestCases are executed.
  * Running JUnit tests over many test classes at once can be sped up remarkably by [organizing test classes into GWTTestSuites](DevGuideJUnitSuites.md).
  * Benchmarks have moved out of com.google.gwt.junit to com.google.gwt.benchmarks; existing client code will need to reorganize imports to the correct package. The Range class has been completely removed in favor of java.lang.Iterable.
  * The setUp/tearDown methods have now been replaced by [gwtSetUp/gwtTearDown](DevGuideJUnitSetUp.md).
  * New -manual web mode testing allows you to target the running test with any browser of your choice. See the [Running your test in manual mode section](DevGuideJUnitCreation.md) in the developer guide for more details.

### Application creator now creates a default css file ###
> If your application's name is MyApp, then the default css file will be named MyApp.css.

### Binary only annotations can be used in translatable source ###
> Of course, this is in addition to source-only annotations.

### Generators Improvements ###
  * Expose Java 1.5 constructs like annotations and generics to Generator authors
  * Generators can now mark Artifacts created by GeneratorContext.tryCreateResource() as being private; these files will be emitted into the module's auxiliary directory. This is a change that ties into the new Linker subsystem introduced in 1.5.
  * Generators can now send custom Artifact instances to the Linker stack via GeneratorContext.commitArtifact(). This change also ties into the new Linker subsystem.

### New 

&lt;rename-to&gt;

 module XML tag ###
> The module XML file now supports an optional attribute "rename-to" which will change the name of the module produced by the compiler. This allows you to do such things as create drop-in replacements for your module by creating various working modules that can be renamed to your standard module name.

### Updated Locale-specific components for standards compliance ###
  * Upgraded DateTimeConstants data to CLDR 1.51, and added the firstDayOfTheWeek and weekendRange formatting symbols.
  * Removed dependency on java.util.Locale in LocalizableGenerator for BCP47 compatibility

### Zero-config Lightweight Performance Metrics ###
Design document can be found [here](http://code.google.com/p/google-web-toolkit/wiki/LightweightMetricsDesign).
  * Provides data about the bootstrap process and RPC subsystem in code paths where user code cannot be added.
  * Supports multiple modules on the same page.
  * Incurs negligible overhead when no statistics collector function is defined.
  * Minimizes observer effects by allowing for data aggregation to be performed in post-processing.
  * Extensible: Users can follow the pattern to instrument their own code with custom metric events.