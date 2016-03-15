# How do I enable assertions? #

In order to use the assert() feature in Java, you must enable them with the `-enableassertions` or `-ea` command line argument.  See the article on the Java website [Programming with Assertions](http://java.sun.com/j2se/1.4.2/docs/guide/lang/assert.html) for more details.  If you are using an IDE, add this argument to the VM argument list.

Only use assertions for debugging purposes, not production logic because assertions will only work under GWT's hosted mode.  They are compiled away by the GWT compiler so do not have any effect in web mode.
