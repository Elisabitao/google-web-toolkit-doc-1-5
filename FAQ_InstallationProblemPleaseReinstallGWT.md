# RuntimeException: Installation problem detected, please reinstall GWT #

When launching the GWT compiler or hosted mode, you may see the following exception.

```
$PP_OFF
.Exception in thread "main" java.lang.ExceptionInInitializerError
Caused by: java.lang.RuntimeException: Installation problem detected, please reinstall GWT
    at com.google.gwt.util.tools.Utility.computeInstallationPath(Utility.java:322)
    at com.google.gwt.util.tools.Utility.getInstallPath(Utility.java:223)
    at com.google.gwt.util.tools.ToolBase.<clinit>(ToolBase.java:55)
Caused by: java.io.IOException: Cannot determine installation directory; apparently not running from a jar
    at com.google.gwt.util.tools.Utility.computeInstallationPath(Utility.java:307)
  ...
```

This message occurs when the runtime binary installation of GWT cannot be located. In general, the runtime binary installation path is expected to be in the same directory where the gwt-dev-`<`platform`>`.jar file is located.

The GWT installation path is embedded in the scripts (created from [applicationCreator](DevGuideApplicationCreator.md) and [projectCreator](DevGuideProjectCreator.md)) as the path specified to the gwt-dev-`<`platform`>`.jar file. If you look in this directory, it should contain not only the gwt-user.jar and gwt-dev-`<`platform`>`.jar files, but also the platform-specific shared library files, such as libgwt\_ll.so or libgwt\_ll.jnilib.

### Workaround ###
Make sure that the path specified to the gwt-dev-`<`platform`>`.jar file on the Java classpath still contains the shared libraries. If not, you may need to re-download and unpack the GWT distribution, making sure your download is complete.

#### Note to contributors ####
If you are a contributor and checked out the source code via svn, you might encounter this problem if you are running GWT from source.

Make sure you have:
  * a binary distribution unpacked or a compiled distribution ready
  * set the system property _gwt.devjar_ configured in your launch configurations as specified in the eclipse/README.txt file.
