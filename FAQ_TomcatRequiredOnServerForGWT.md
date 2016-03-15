# Do I need to run Tomcat on my server to use GWT? #

Not at all! It's true that GWT's hosted mode can use an embedded Tomcat instance to serve up your Java Servlets if that is convenient to you, but you are not required to use Tomcat, J2EE, or even Java at all if you choose not to.

By passing the `-noserver` option to the GWT hosted mode script, you can allow your GWT hosted mode shell to connect to a different server when making server connections for data. For more information, see ["How do I use my own server in hosted mode instead of GWT's built-in Tomcat instance?"](FAQ_HostedModeNoServer.md)