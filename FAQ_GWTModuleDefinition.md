# What is a GWT Module? #

A GWT "module" is simply an encapsulation of functionality.  It shares some similarities with a Java package but is not the same thing.
<a href='Hidden comment: 
It"s important to understand GWT modules to avoid confusion, although modules are actually very simple to understand and work with.
'></a>

A GWT module is named similarly to a Java package in that it follows the usual dotted-path naming convention.
For example, most of the standard GWT modules are located underneath "com.google.gwt" However, the similarity between GWT modules and Java packages ends with this naming convention.

A module is defined by an XML descriptor file ending with the extension ".gwt.xml", and the name of that file determines the name of the module.
For example, if you have a file named `src/com/mycompany/apps/MyApplication.gwt.xml`, then that will create a GWT module named `com.mycompany.apps.MyApplication`.  The contents of the `.gwt.xml` file specify the precise list of Java classes and other resources that are included in the GWT module.
<a href='Hidden comment: 
Modules defined in this way are used throughout GWT, and so it"s important to understand how they work.  It is very easy to create a GWT module for the application you"re working on (and in fact GWT"s tools automate this process), but not understanding all the nuances of GWT modules can lead to confusion.  For example, in order to use [http://google-web-toolkit.googlecode.com/svn/javadoc/1.6/com/google/gwt/junit/client/package-summary.html GWT"s JUnit testing infrastructure] properly, you must understand how to create a GWT module, since an incorrectly-constructed module will prevent your tests from working.  Similarly, not understanding and including the proper standard GWT modules for the functionality you are trying to use (such as I18N or JSON) can prevent your application from working.
'></a>

## Related ##
  * For details on how to define a GWT module, see [Modules](DevGuideModules.md).
  * For a list of the standard GWT modules, see [How do I know which GWT modules I need to inherit?](FAQ_GWTModuleInheritance.md).