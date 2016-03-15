# Deployment in Web Mode #

As you move from development into end-to-end testing and production, you will begin to interact with your application in web mode more often. _Web mode_ refers to accessing your application from a normal browser -- where it runs as pure JavaScript -- as it is ultimately intended to be deployed. Web mode demonstrates what makes GWT unique: as we've seen previously, the GWT compiler translates your GWT application code into JavaScript, which runs directly in the browser without plug-ins or a JVM.

To run you GWT application in web mode, you can use one of the following methods:

  * Run the main class `com.google.gwt.dev.GWTCompiler` using `java` from the command-line.
  * Use the convenience shell script `<project-name>-compile` created from the `applicationCreator` script when your project was created.
  * Use the _Compile/Browse_ button available in the hosted browser window.  Your desktop's default browser will be launched with a URL to fetch your compiled application.

The compiled output will go into the subdirectory `www` if you use the `<module>-compile` convenience shell script or compile from hosted mode launched by the `<module>-shell` script.  The client-side code is placed in a subdirectory underneath `www` named the same as your Java module's package name.

You can control the destination of the compiler output using the `-out` command-line argument.  See the [GWT Compile Script](DevGuideModuleCompileScript.md) section for the full list of command-line options.  Also see the [GWT Compiler](DevGuideJavaToJavaScriptCompiler.md) section for more details about the compiler itself.
If you launched the application in web mode from the Hosted Mode browser using the Compile/Browse button, a new default web browser window will automatically be opened to run the application.  Loading the file named 

&lt;module&gt;

_`.html` into a web browser will allow you to run the JavaScript version of your application._


> _Tip: The Google Web Toolkit does not endorse any particular backend technology and can integrate with any arbitrary backend technology that is best suited for your application.  See the following FAQs for common questions regarding integrating and testing your GWT application code with your custom server-side technology:
  * [Do I need to run Tomcat on my server?](FAQ_TomcatRequiredOnServerForGWT.md)
  * [How do I use my own server in hosted mode instead of GWT's built-in Tomcat instance?](FAQ_HostedModeNoServer.md)
  * [Deploying to a WAR file](FAQ_PackageAppInWARFile.md)
  * [Can I use GWT with my favorite server-side templating tool?  ](FAQ_GWTWithServerSideTemplatingTool.md)
  * [How do I make a call to the server if I am not using the GWT RPC? ](FAQ_ServerCallWithoutRPC.md)
>_

### See Also ###

  * [Getting Started Tutorial - Web mode](GettingStartedWebMode.md)

  * [Example deployment with Tomcat](DevGuideRPCDeployment.md)
