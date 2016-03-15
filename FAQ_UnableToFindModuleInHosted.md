# Unable to find type 'com.foo.client.MyApp' #
When running in hosted mode, receive the error: Unable to find type 'com.foo.client.MyApp'.

```
$PP_OFF
Starting HTTP on port 8888
Finding entry point classes
  Unable to find type 'com.google.gwt.sample.stockwatcher.client.StockWatcher'
    Hint: Check that the type name 'com.google.gwt.sample.stockwatcher.client.StockWatcher' is really what you meant
    Hint: Check that your classpath includes all required source roots
Failure to load module 'com.google.gwt.sample.stockwatcher.StockWatcher'    
```

One of the most common problems when creating launch configurations for the first time in GWT is to omit the application source code path from the Java classpath. The [GWT compiler](DevGuideJavaToJavaScriptCompiler.md) and [hosted mode](DevGuideHostedMode.md) shell rely on the module source code to build your application, and both use the Java classpath to find the .java source files.

### Workaround ###
If you are using Eclipse, open the launch configuration, navigate to the Classpath tab, and use the Advanced button to add the `src` folders to the classpath.

If you are launching from the command line, add the `src` folders to the list of paths following the `-cp` argument to the Java command.
