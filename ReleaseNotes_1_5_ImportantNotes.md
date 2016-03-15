# Important notes #

If you have already been using GWT for a while, there are some changes in 1.5 that might affect your development. They aren't really breaking API changes, but they could trip you up, so we wanted to call them out clearly.

### GWT 1.5 requires Java 5 or later ###
You probably have already been using a Java 5 SDK with GWT already, but if not, you'll need to make the switch now.

### You'll want to start using generics and annotations... ###
...especially for [RPC](DevGuideRemoteProcedureCalls.md), [internationalization](DevGuideInternationalization.md), and [image bundles](DevGuideImageBundles.md). All the old javadoc-style metadata such as `@gwt.typeArgs` has been superceded by proper annotations. Future versions of GWT will no longer honor the javadoc style of annotations, so expect to see a lot of log warnings  reminding you to update your code to use annotations.

For RPC, you'll need to replace `@gwt.typeArgs` in your javadoc with proper Java generics.

For internationalization, there is a new, rich set of annotations. See the "Annotation Types Summary" section for [Constants and Messages](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/package-summary.html).

For image bundles, replace `@gwt.resource` in your javadoc with the [@Resource](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/ImageBundle.Resource.html) annotation.

Although we strongly suggest that you update your project code to use the standard Java 1.5 style annotations, you can use the new `-Dgwt.nowarn.metadata` flag to turn off warnings about using deprecated metadata annotations if you're hard pressed to update your code. If your application uses a lot of metadata, this flag might help improve compilation and hosted mode startup times since generating loads of warning messages can be costly. The right solution, however, is to update your code.

### Java `long` types cannot be passed to JSNI methods ###
In GWT 1.5, the Java `long` type now works correctly, giving you the full proper range of a 64-bit integer. However, due to JavaScript's lack of true 64-bit integral types, long is represented as a pair of 32-bit integers and will not work properly with JavaScript's standard math operators. The GWT 1.5 compiler will issue errors if you attempt to pass `long` values into JSNI. In cases where you do not need the full range of `long`, changing the type to `double` is the recommended work around. In fact, if you are using a `long` returned from [System.currentTimeMillis()](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#currentTimeMillis()) we recommend that you use the new [Duration](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/Duration.html) class instead. For JSNI methods that treat `long` values as opaque objects (not performing any math), you can suppress the compiler error with the [UnsafeNativeLong](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/UnsafeNativeLong.html) annotation. We do, however, recommend that you avoid this route as it is very easy to forget that the value is not a numeric type within the JSNI method.

### Expect to use the -Xmx JVM flag ###
The GWT compiler and hosted mode use more memory internally now. Some projects will need to increase JVM max memory to successfully compile, others may see improved compile times by increasing the limit. For example, `-Xmx512M` would set the heap max to 512 megabytes.

### Assertions are always on in hosted mode ###
GWT client code running in hosted mode always runs with assertions enabled now. The GWT libraries are increasingly using assertions to validate proper use of API methods rather than explicitly checking parameters via if/then/throw sequences. Using assertions in this manner helps you catch runtime bugs in hosted mode without forcing overhead in [web mode](DevGuideWebMode.md). (You can choose to compile-in assertions in web mode, too, now. See [the command-line](DevGuideModuleCompileScript.md) for `GWTCompiler` for more details.)

### Hosted mode works on OS X 10.4 and 10.5; requires Safari 3.0 ###
The same GWT distribution for Mac OS X now works on Tiger (10.4) and Leopard (10.5). The new implementation relies on using the system's available install of Safari 3.0 or later rather than bundling its own version of WebKit.
