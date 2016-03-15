# Unable to find module on your classpath #
Although you have successfully compiled the project class files in an IDE, when invoking the GWT compiler for the first time on a new project using one of the APIs provided with the GWT Google API, you come across an error similar to the following:
```
$PP_OFF
Loading module 'com.example.gearsTest'
   Loading inherited module 'com.google.gwt.gears.Gears'
      [ERROR] Unable to find 'com/google/gwt/gears/Gears.gwt.xml' on your
classpath; could be a typo, or maybe you forgot to include a classpath
entry for source?
   [ERROR] Line 6: Unexpected exception while processing element 'inherits'
com.google.gwt.core.ext.UnableToCompleteException: (see previous log entries)
...
```

### Workaround ###
When using any third party API, make sure that the GWT compiler can access the third party source code through the Java classpath.

If you invoke the GWT compiler with the [compile](DevGuideModuleCompileScript.md) script, then modify it by adding the `gwt-google-apis.jar` file to the `-cp` argument:
```
#!/bin/sh
APPDIR=`dirname $0`;
GWT_HOME=/Users/zundel/Documents/workspace2/GWT/build/staging/gwt-mac-0.0.0/
java -cp "$APPDIR/src:$APPDIR/bin:$GWT_HOME/gwt-user.jar:$GWT_HOME/gwt-dev-mac.jar:\ 
 /Users/zundel/Documents/workspace2/gwt-google-apis/build/lib/gwt-google-apis.jar" \
com.google.gwt.dev.GWTCompiler -out "$APPDIR/www" "$@" com.example.gearsTest;
```
