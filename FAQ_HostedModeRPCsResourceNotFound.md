# Resource not found #
When testing a new RPC interface in hosted mode, receive a "resource not found" error.

```
$PP_OFF
The development shell servlet received a request for 'rpcs/myService' in module 'com.example.RPCExample' 
   Resource not found: rpcs/myService
```

The servlet engine could not find the server-side definition of your RPC method.

### Troubleshooting checklist ###
  * Is the server side code defined and in the appropriate directory?
> By convention, services are located in the _server_ directory in the [standard project structure.](DevGuideDirectoriesPackageConventions.md)

  * Is the server code path listed in the module XML file (_myApp_.gwt.xml)?
```
<!-- Example servlet loaded into hosted mode tomcat       -->
<servlet path="/myService" class="com.example.server.MyServiceImpl" />
```

  * Have you complied the server side code?
> Of significant note, if you are using a plain text editor and then running your application from command line with the [hosted mode shell](DevGuideModuleHostedModeScript.md) and [compile](DevGuideModuleCompileScript.md) scripts, these scripts do **NOT** compile server-side code.

  * Are the compiled server classes are available to the hosted mode shell?
> If your server classes are in a separate jar file or directory tree, when launching the hosted mode shell you may need to modify the classpath (for example, by editing the hosted mode shell script).
