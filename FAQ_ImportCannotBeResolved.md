#summary GWT compiler cannot resolve import because module XML file does not point to resource.

# The import cannot be resolved #
Although you have successfully compiled the project class files in an IDE, when invoking the GWT compiler for the first time on a new project using one of the APIs provided with the GWT Google API, you come across an error similar to the following:

```
$PP_OFF
$ ./gearsTest-compile
Analyzing source in module 'com.example.gearsTest'
   [ERROR] Errors in '/Users/zundel/Documents/workspace2/galgwt-issue3/src/com/example/client/gearsTest.java'
      [ERROR] Line 9:  The import com.google.gwt.gears cannot be resolved
      [ERROR] Line 26:  Gears cannot be resolved
```

The problem is that there is no package `com.google.gwt.gears` on the module source path.

### Workaround ###
This can be resolved by updating the module XML file (.gwt.xml) in either of two ways.
  * Add a `<`source`>` tag that adds the package to the module source path.
  * If your third party library provides one, add an `<`inherits`>` tag.
For using the Gears package in GWT Google API, the appropriate line is:
```
   <inherits name='com.google.gwt.gears.Gears'>
```
