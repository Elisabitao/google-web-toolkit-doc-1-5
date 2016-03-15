# Archiving and reusing GWT Modules #

To archive and reuse a GWT module, follow the steps below:

  1. **Create a new JAR file** that contains the module you wish to archive, including all source code for the module (including Module XML file and public resources).
  1. **Add the newly created JAR to the project classpath** in which you would like to reuse the previous module code. Also add the new JAR to any -shell, -compile, or other run configurations classpaths related to the inheriting project.
  1. **Add an `<inherits>` tag** referencing the archived module XML in the inheriting project module XML file.

You might notice a few differences between archiving and reusing a GWT module versus archiving and reusing an standard Java project. These differences exist for good reason, and merit an explanation.

The first difference is that archiving a GWT module into a JAR requires you to include all the source code to the project you want to reuse. In standard Java, usually all that is required to archive and reuse code are the associated .class files. The reason why you need the source code to reuse a module in GWT is because the GWT compiler works from Java source code, rather than the compiled binary. This means in order to reference another GWT module, the source code must be available for the GWT compiler to find and translate it into JavaScript.

The second difference is the last extra step that mentions including an `<inherits>` tag in the inheriting project module XML file that references the archived module XML file. The reason why this is necessary is to tell the GWT compiler which other resources you are reusing in your application, as well as where to find the associated source code for those resources.

This process of archiving and reusing GWT modules is the same as the one used for [inheriting standard GWT modules](FAQ_GWTModuleInheritance.md). Using the standard GWT modules as an example, we are inheriting from the archived gwt-user.jar, which includes the source code for all the GWT widgets and a User.gwt.xml file, which is referenced via the `<inherits>` tag in our own project's module XML file.