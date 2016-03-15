# Help! I'm having problems with eval() in my JSNI method! #

Occasionally you might want to use this idiom for a GWT class method:

```
public static native String myMethod(String arg) /*-{
    eval("var myVar = 'arg is ' + arg;");
    return myVar;
}-*/;
```

The code above will work in hosted mode, but not in web mode.  The reason is that when GWT compiles the Java source code to JavaScript, it obfuscates variable names.  In this case, it will change the name of the `arg` variable.  However, GWT can't see into JavaScript string literals in a JSNI method, and so it can't update the corresponding embedded references to `arg` to also use the new varname.

The fix is to tweak the `eval()` statement so that the variable names are visible to the GWT compiler:

```
public static native String myMethod(String arg) /*-{
    eval("var myVar = 'arg is " + arg + "';");
    return myVar;
}-*/;
```