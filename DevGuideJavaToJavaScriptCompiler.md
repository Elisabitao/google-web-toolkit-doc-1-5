# GWT Compiler #

The heart of GWT is a compiler that converts Java source into JavaScript, transforming your working Java application into an equivalent JavaScript application. Generally speaking, if your GWT application compiles and runs in [hosted mode](DevGuideHostedMode.md) as you expect and GWT compiles your application into JavaScript output without complaint, then your application will work the same way in a web browser as it did in hosted mode.

The GWT compiler supports the vast majority of the Java language. The [GWT runtime library](DevGuideJavaCompatibility.md) emulates a relevant subset of the Java runtime library.  If a JRE class or method is not supported, the compiler will emit an error.

You can run the compiler with the name of the module you want to compile in one of the following manners:

  * Run the main class `com.google.gwt.dev.GWTCompiler` using `java` from the command-line.
  * If you used the 'applicationCreator' script to create your project, you can use the generated convenience shell script `<project-name>-compile`.
  * Use the _Compile/Browse_ button from inside of hosted mode.

Once compilation completes sucessfully, directories will be created containing the JavaScript implementation of your project.  The compiler will create one directory for each module it compiles.

```
$PP_OFF
$ Foo-compile
Output will be written into ./www/com.googlecode.example.Foo
Compilation succeeded
$ ls www
com.googlecode.example.Foo/
```

Inside that directory is the implementation for your module in JavaScript.

```
$PP_OFF
065830997FFBD2DC13A986D8939F5704.cache.html
065830997FFBD2DC13A986D8939F5704.cache.js
065830997FFBD2DC13A986D8939F5704.cache.xml
0791B818FB6E04B9F03E69AE31715E7D.cache.html
0791B818FB6E04B9F03E69AE31715E7D.cache.js
0791B818FB6E04B9F03E69AE31715E7D.cache.xml
10B80E29ACBFA1FBEF8A475830840009.cache.html
10B80E29ACBFA1FBEF8A475830840009.cache.js
10B80E29ACBFA1FBEF8A475830840009.cache.xml
439FF78FFF7914AF6A60AD1111599BC0.cache.html
439FF78FFF7914AF6A60AD1111599BC0.cache.js
439FF78FFF7914AF6A60AD1111599BC0.cache.xml
B7B701C677F4C1BBA4B386753292D960.cache.html
B7B701C677F4C1BBA4B386753292D960.cache.js
B7B701C677F4C1BBA4B386753292D960.cache.xml
Foo.html
clear.cache.gif
com.googlecode.example.Foo-xs.nocache.js
com.googlecode.example.Foo.nocache.js
gwt.js
history.html
hosted.html
```

In the above example, the module name is **Foo** and the filename you should point your application host HTML page is **Foo.html**. To run your application in web mode, simply point your browser to the `Foo.html` page.

The full list of command-line options is the same as the [&lt;module&gt;-compile script](DevGuideModuleCompileScript.md) options.
