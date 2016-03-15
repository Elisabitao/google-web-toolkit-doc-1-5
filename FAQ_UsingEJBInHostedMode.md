# How do I use EJBs in hosted mode? #

GWT provides the -noserver option to the hosted mode shell script for precisely this purpose.

The -noserver option instructs hosted mode to not start the embedded Tomcat instance.  In its place, you would run the J2EE container of your choice and simply use that in place of the embedded Tomcat instance.

For more information see [How do I use my own server in hosted mode instead of GWT's built-in Tomcat instance?](FAQ_HostedModeNoServer.md)
