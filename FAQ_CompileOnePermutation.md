# Can I speed up the GWT compiler? #

If you are compiling a large application, you may find that compiling to web mode takes a long time.  One issue is that the compiler actually builds several versions of your application based on client properties for locale and browser.  For deployment, this is crucial, but for everyday development, you are probably only using a single browser and locale.  If that is the case, then you can take a shortcut and compile only a single permutation during the development cycle.

Creating this shortcut setup requires creating a new module and manipulating the client properties. What you need to do is to create a module that inherits from your existing module that specifies exact values for client properties that you want to nail down. Let's use `Hello` as an example...

  1. Create a module called `HelloFirefox` that inherits Hello
  1. Use `<set-property>` in the `HelloFirefox` module that explicitly sets a value for
the `user.agent` client property. (See `<define-property>` in `UserAgent.gwt.xml` for
the possible values.)
  1. Update your host html to refer to the `HelloFirefox` module rather than the Hello
module.
  1. Compile the `HelloFirefox` module instead of the `Hello` module.
  1. Look in the www/

&lt;modulename&gt;

/ directory: there should be only one permutation compiled.

This example uses "gecko", which I tested on my older Firefox 1.0.7.  First, the GWT module file:

```
<module>
	<inherits name="com.google.gwt.sample.hello.Hello"/>
	<set-property name="user.agent" value="gecko"/>
</module>
```

Next, the host HTML file:

```
<html>
  <head>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <title>Hello</title>
  </head>
  <body bgcolor="white"> 
    <script type="text/javascript" language="javascript" src="com.google.gwt.sample.hello.HelloFirefox.nocache.js"></script>
  </body>
</html>

```

You can do the same thing for `locale` or any other client property. The subsystem  that generates all those permutations is completely extensible, so this technique is a general one.

To avoid keeping many nearly duplicate module files, the _[rename-to](http://code.google.com/docreader/#p(google-web-toolkit-doc-1-5)s(google-web-toolkit-doc-1-5)t(DevGuideModuleXml))_ attribute on the `<module>` tag to create a working copy of the GWT application that is specific to the single-browser single-locale development mode. You can keep two module XML files in parallel, one for solid development to be tested on all browsers, and another working copy with most permutations suppressed for draft code.

Hopefully this example also starts to show that the idea of a module isn't as
trivial as it might seem at first. Modules play an important role letting you
determine what exactly you're trying to build. You can have as many modules as you
want for different configurations for the same code base. You can imagine module
variations like "MySuperBigModuleWithDebuggingAndLoggingTurnedOn.gwt.xml".
