# Deferred Binding using Generators #

The second technique for deferred binding consists of using _generators_.  Generators are classes that are invoked by the GWT compiler to generate a Java implementation of a class during compilation. When compiling to web mode, this generated implementation is directly translated to one of the versions of your application in JavaScript code that a client will download based on its browser environment.

The following is an example of how a deferred binding generator is specified to the compiler in the [module XML file](DevGuideModuleXml.md) hierarchy for the `RemoteService` class - used for GWT-RPC:

  * Top level 

&lt;module&gt;

_.gwt.xml_**inherits**_[com.google.gwt.user.User](http://google-web-toolkit.googlecode.com/svn/trunk/user/src/com/google/gwt/user/User.gwt.xml)
  * [com/google/gwt/user/User.gwt.xml](http://google-web-toolkit.googlecode.com/svn/trunk/user/src/com/google/gwt/user/User.gwt.xml)_**inherits**_[com.googl.gwt.user.RemoteService](http://google-web-toolkit.googlecode.com/svn/trunk/user/src/com/google/gwt/user/RemoteService.gwt.xml)
  * [com/google/gwt/user/RemoteService.gwt.xml](http://google-web-toolkit.googlecode.com/svn/trunk/user/src/com/google/gwt/user/RemoteService.gwt.xml)_**contains**_`<generates-with>` elements to define deferred binding rules for the `RemoteService` class._

## Generator Configuration in Module XML ##

The XML element `<generate-with>` tells the compiler to use a `Generator` class. Here are the contents of the `RemoteService.gwt.xml` file relevant to deferred binding:

```
<module>

 <!--  ... other configuration omitted ... -->
	
 <!-- Default warning for non-static, final fields enabled -->
 <set-property name="gwt.suppressNonStaticFinalFieldWarnings" value="false" />

 <generate-with class="com.google.gwt.user.rebind.rpc.ServiceInterfaceProxyGenerator">
   <when-type-assignable class="com.google.gwt.user.client.rpc.RemoteService" />
 </generate-with>
</module>
```

These directives instruct the GWT compiler to invoke methods in a [Generator](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/ext/Generator.html) subclass (`ServiceInterfaceProxyGenerator`) in order to generate special code when the deferred binding mechanism [GWT.create()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class)) is encountered while compiling.  In this case, if the [GWT.create()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class)) call references an instance of `RemoteService` or one of its subclasses, the `ServiceInterfaceProxyGenerator`'s generate()` method will be invoked.

## Generator Implementation ##

Defining a subclass of the `Generator` class is much like defining a plug-in to the GWT compiler.  The `Generator` gets called to generate a Java class definition before the Java to JavaScript conversion occurs.  The implementation consists of one method that must output Java code to a file and return the name of the generated class as a string.

The following code shows the `Generator` that is responsible for deferred binding of a `RemoteService` interface:

```
/**
 * Generator for producing the asynchronous version of a
 * {@link com.google.gwt.user.client.rpc.RemoteService RemoteService} interface.
 */
public class ServiceInterfaceProxyGenerator extends Generator {

  /**
   * Generate a default constructible subclass of the requested type. The
   * generator throws <code>UnableToCompleteException</code> if for any reason
   * it cannot provide a substitute class
   * 
   * @return the name of a subclass to substitute for the requested class, or
   *         return <code>null</code> to cause the requested type itself to be
   *         used
   * 
   */
  public String generate(TreeLogger logger, GeneratorContext ctx,
      String requestedClass) throws UnableToCompleteException {

    TypeOracle typeOracle = ctx.getTypeOracle();
    assert (typeOracle != null);

    JClassType remoteService = typeOracle.findType(requestedClass);
    if (remoteService == null) {
      logger.log(TreeLogger.ERROR, "Unable to find metadata for type '"
          + requestedClass + "'", null);
      throw new UnableToCompleteException();
    }

    if (remoteService.isInterface() == null) {
      logger.log(TreeLogger.ERROR, remoteService.getQualifiedSourceName()
          + " is not an interface", null);
      throw new UnableToCompleteException();
    }

    ProxyCreator proxyCreator = new ProxyCreator(remoteService);

    TreeLogger proxyLogger = logger.branch(TreeLogger.DEBUG,
        "Generating client proxy for remote service interface '"
            + remoteService.getQualifiedSourceName() + "'", null);

    return proxyCreator.create(proxyLogger, ctx);
  }
}
```

The `typeOracle` is an object that contains information about the Java code that has already been parsed that the generator may need to consult. In this case, the `generate()` method checks it arguments and the passes off the bulk of the work to another class (`ProxyCreator`).
