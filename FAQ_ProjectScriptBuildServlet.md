# Why don't the project scripts build my Servlets? #

GWT takes a "hands-off" philosophy for components it doesn't need to interact with.  Even though GWT includes an optional Tomcat container in its hosted mode as a convenience, GWT doesn't presume to know what it should do with your server code.  As a result, the project scripts focus on your translatable JavaScript, and you must compile your Servlets (or other Java code) on your own, using your favorite build tool.
