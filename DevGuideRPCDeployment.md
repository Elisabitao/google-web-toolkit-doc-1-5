# Deploying RPC #

The GWT hosted mode embedded Tomcat instance is only intended for debugging your application. If you use GWT RPC, you can configure your GWT RPC servlets to the embedded Tomcat instance for debugging, but once your application is ready for deployment, it is time to deploy to a production server.  When you do so, you will need to select a servlet container (also called a web container or web engine) to run the backend.  GWT does not provide a servlet container to use in production, but there are many different products available.  The following lists just a few commercial and non-commercial offerings:

  * [Apache Tomcat](http://tomcat.apache.org/)
  * [Jetty](http://jetty.mortbay.com/)
  * [JBoss](http://labs.jboss.com/)
  * [IBM Web Sphere](http://www-306.ibm.com/software/websphere/)
  * [Oracle Application Server](http://www.oracle.com/appserver/index.html)

If you want to continue debugging in hosted mode but your server-side requirements have become more than the embedded Tomcat server can handle, you can use your own [production server or custom development server in hosted mode](FAQ_HostedModeNoServer.md) as well. After setting up your server, simply start up hosted mode with the `-noserver` option to disable GWT's builtin servlet container.

In the examples below, we'll show how to deploy your server-side components to a production server using Apache HTTPD and Apache Tomcat as a web engine.  Although there are many different implementations of web servers and servlet containers, the [Servlet API Specifications](http://java.sun.com/products/servlet/download.html) define a standard structure for project directories which most web engines follow, so we will follow the same specification here.

> _Tip: For the latest servlet specification documentation see the [Java Community Process](http://jcp.org/) website.  It will describe the general workings of the servlet and the conventions used in configuring your servlets for deployment._

## Simple Example with Apache Tomcat ##

The simplest way to deploy your application is to use a single server for both your static content and your servlet classes.   These steps assume that you created your project with [applicationCreator](DevGuideApplicationCreator.md):

  1. **Compile** : Compile your code using the 

&lt;module&gt;

_`-compile` file created by [applicationCreator](DevGuideApplicationCreator.md). If the compile is sucessful, the compiler output will be placed in a folder called `www/<package>.<modulename>`.
  1. **Create a staging area** : Create a temporary folder where you can stage your files.  In this example, we will call the folder `production`.
  1. **Copy compiled files to the staging area** : Copy the content files from `www/<package>.<modulename>` to the `production` staging area folder.  You should now have files named `*.nocache.js`, `*.cache.js`, `*.cache.xml`, `*.cache.html` and others in the `production` staging area folder.
  1. **Create WEB-INF subdirectory structure** : In the `production` directory,  create a new folder called `WEB-INF`. Inside this `WEB-INF` folder create 2 new folders called `classes` and `lib`.
  1. **Create a `web.xml` file** : In the `WEB-INF` folder create file named `web.xml` file.  This configures the servlet container._

The following example shows what the contents of a `web.xml` file may look like that configures a Foo servlet:

```
<?xml version="1.0" encoding="UTF-8"?>

<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">

 <!-- Standard Action Servlet Configuration -->
 <servlet>
   <servlet-name>Foo</servlet-name>
   <servlet-class>com.example.foo.server.fooImpl</servlet-class>
 </servlet>

 <!-- Standard Action Servlet Mapping -->
 <servlet-mapping>
   <servlet-name>Foo</servlet-name>
   <url-pattern>/Foo</url-pattern>
 </servlet-mapping>

</web-app>
```
In a GWT application, you would need to make sure that the `<url-pattern>` value matches the suffix you used in the `setServiceEntryPoint(URL)` call when creating the GWT-RPC service (the same path you would use in the `<module>.gwt.xml` to configure your servlet for hosted mode.)
  1. **Compile your servlet classes** : By default GWT does not compile the server side classes.  Use `javac` or your IDE of choice to compile the server-side code.  You should end up with a directory structure similar to `com/example/foo/server/...`, possibly in the `bin` folder of your project, that contains the compiled `.class` files.
  1. **Copy the servlet classes** : Copy the compiled class files from your project to the `/WEB-INF/classes` folder in the staging area. Make sure you include all of the subdirectories as well.
  1. **Add the GWT servlet library** : Copy the `gwt-servlet.jar` from the GWT distribution to the `/WEB-INF/lib` folder.  You should not put any of the other jar files from the GWT distribution in this folder.
  1. **Copy any third party libraries** :  If your project uses any third party libraries, make sure you copy those `.jar` files to the `/WEB-INF/lib` folder as well.
  1. **Create a .zip archive** : Use your favorite tool to compress the staging area into an archive.  Make sure to preserve the directory structure, but do not include the root `production/` folder.
  1. **Rename the file to end in `.war`** : Web engines deploy applications using Web Archive Repositories (WAR files).  By renaming the `.zip` file you just created by changing the extension to `.war`, you'll have converted the archive file to a Web Archive Repository that can be deployed to Tomcat.
  1. **Copy to Tomcat** : Copy your `.war` file into Tomcat's `/webapps` folder.
If you have default configuration settings it should automatically unzip the .war file.

If Tomcat is in its default configuration to run on port 8080, you should be able to run your application by entering  the url  `http://<hostname>:8080/Foo/Foo.html` into your web browser.

If you encounter any problems, take look in the Tomcat log file, which can be found in the `logs` directory of your Tomcat installation.  If your web pages display but the RPC calls don't seem to be going through, try turning on access logging on Tomcat.  You may find that the URL used by the client side has not been registered by Tomcat, or that there is a misconfiguration between the URL path set in the `setServiceEntryPoint(URL)` call when declaring your RPC service and the `<url-pattern>` URL mapping the the `web.xml` file.


## Using Tomcat with Apache HTTPD and a proxy ##

The above example is a good way to test your application from client to server, but may not be the best production setup.  For one, the Jakarta/Tomcat server is not the fastest web server for static files.  Also, you may already have a web infrastructure setup for serving static content that you would like to leverage.  The following example will show you how to split the web engine deployment from from the static content.

When you run the 

&lt;module&gt;

_`-compile`, the output generated by the GWT compiler in the `www/` directory is your static content.  Copy this to a subdirectory in your apache configuration's document root.  We will call that directory `/var/www/doc/Foo` and the resulting URL is `http://www.example.com/foo-app/Foo.html`._

Let's assume that the tomcat server is on a different host named `servlet.example.com`.  In this case, there is going to be a problem.  Browsers' Single Origin Policy (SOP) will prevent connections to ports or machines that differ from the original URL.  The strategy we are going to use to satisfy the SOP is to configure Apache to proxy a URL to another URL.

Specific directions for configuring Apache and Tomcat for such a proxy setup are at [the Apache website](http://tomcat.apache.org/tomcat-6.0-doc/proxy-howto.html).  The general idea is to setup Apache to redirect from a sub-directory of the foo-app project to the other server such that:

`http://foo.example.com/foo-app/rpcs/ --> http://servlet.example.com:8080/Foo/`

The following Apache configuration sets up such a rule using a Proxy:

```
ProxyPass /Foo/rpcs/  http://servlet.example.com:8080/Foo/
ProxyPassReverse /Foo/rpcs/  http://servlet.example.com:8080/Foo/
```


Inside your client-side code, you will need to use 2 directories to map the proxy director to the server.  This proxy setup will strip off one level of directory on the client side, so there is an added twist to setting up the `web.xml` file.  Suppose you have set the proxy directory to be `/rpcs/` and are using a backend servlet named `servletName`.  In `setServiceEntryPoint(URL)`, you should append a path `rpcs/serviceName` to the base URL.  In the Tomcat configuration, you should set the `<url-mapping>` tag to map the servlet as simply `serviceName`.


Now, configure your WAR file using the steps in the example for the previous section above with the following changes:

  * Change the `<url-mapping>` as mentioned above.
  * You can omit copying the contents of the `www/` directory (those files should be deployed to the Apache server instead)

## Other Deployment Methods ##

It should be mentioned that these are just two examples of deployment scenarios.  There are many ways to configure your application for deployment:

  * Many developers use [Ant](http://ant.apache.org/) or a script to automate the deployment of the `.war` file.
  * Some Web Engines take special directives in `web.xml` that need to be taken into consideration.
  * Some web servers have optimized ways to route servlet calls to a servlet engine (e.g. mod\_jk on Apache)
  * Some Web servers and Web Engines have facilities for replication so you can load balance your servlet across many nodes.
