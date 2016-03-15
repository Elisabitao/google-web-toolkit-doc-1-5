# How do I package a GWT application into a WAR file? #

The short, easy answer to this question is:  any way you like.  To any J2EE application, the output of the GWT Compiler is purely static content.  The fact that the files represent dynamic code that runs in the JavaScript browser is transparent to the J2EE server.  You can put your compiled GWT files anywhere in your WAR file that you like.  See [the Developer's Guide](DevGuideRPCDeployment.md) for an example of how to deploy onto Tomcat.

However, since GWT is indeed a dynamic AJAX application on the client, creators of WAR files need to take special care to package the GWT files so that they can be located by the bootstrap process.  The full details of how to do this are described in the topic ["How do I change the location of my cache/nocache HTML files?"](FAQ_ChangeLocationGWTApplicationFiles.md).

As long as the guidelines in that article are followed, the GWT application files can be placed anywhere in the WAR structure.  No changes need to be made to the `web.xml` file, nothing needs to be placed in the `WEB-INF` directory, and nothing needs to be on the classpath.

**Considerations for GWT RPC Users**
The only case in which users do need to worry about the `WEB-INF` directory is where they have chosen to use GWT's RPC mechanism.  Such users will have one or more Servlet classes that need to be configured.  Even in this scenario, it is still straightforward to set up:

  * The `gwt-servlet.jar` file needs to be on the classpath -- that is, in `WEB-INF/lib` directory.  (This JAR file provides the server-side classes required by the GWT RPC mechanism.  Note: the gwt-servlet.jar file **must be in the `WEB-INF/lib`** of each WAR file that uses GWT; you cannot have a single copy on the system classpath for all Servlet contexts.)
  * The application's RPC Servlet classes (and any dependencies) must be in the `WEB-INF/classes` directory or the `WEB-INF/lib` directory as appropriate. (This is standard J2EE practice.)
  * Each RPC Servlet must be configured in `WEB-INF/web.xml` to have the appropriate Servlet-Mapping.  That is, if the GWT application expects to see an RPC Servlet at the URL /servlet-context/gwtrpc, then the Servlet mapping in `web.xml` must properly configure the RPC class to appear at the `/gwtrpc` path.  (This is also standard J2EE practice.)


GWT does not require any fancy footwork to deploy in a J2EE WAR.  It does, however, require that the person building the WAR take the usual care in setting it up, plus a little extra consideration of how GWT bootstraps an application.