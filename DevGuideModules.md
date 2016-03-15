# Modules #

Individual units of GWT configuration are XML files called _modules_. A module bundles together all the configuration settings that your GWT project needs:

  * Inherited modules
  * An entry point application class name; these are optional, although any module referred to in HTML must have at least one entry-point class specified
  * Source path entries
  * Public path entries
  * Deferred binding rules, including property providers and class generators

Modules may appear in any package in your classpath, although it is strongly recommended that they appear in the root package of a [standard project layout](DevGuideProjectStructure.md).

## Entry-Point Classes ##
A module entry-point is any class that is assignable to [EntryPoint](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/EntryPoint.html) and that can be constructed without parameters. When a module is loaded, every entry point class is instantiated and its [EntryPoint.onModuleLoad()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/EntryPoint.html#onModuleLoad()) method gets called.

## Source Path ##
Modules can specify which subpackages contain translatable _source_, causing the named package and its subpackages to be added to the _source path_. Only files found on the source path are candidates to be translated into JavaScript, making it possible to mix [client-side](DevGuideClientSide.md) and [server-side](DevGuideServerSide.md) code together in the same classpath without conflict.
When module inherit other modules, their source paths are combined so that each module will have access to the translatable source it requires.

> _The default source path is the `client` subpackage underneath where the [Module XML File](DevGuideModuleXml.md) is stored._

## Public Path ##
Modules can specify which subpackages are _public_, causing the named package and its subpackages to be added to the _public path_. The public path is the place in your project where static resources such as HTML files, images, and third party javascript libraries are stored.  When you compile your application into JavaScript, all the files that can be found on your public path are copied to the module's output directory. The net effect is that user-visible URLs need not include a full package name.  When module inherit other modules, their public paths are combined so that each module will have access to the static resources it expects.

> _The default public path is the `public` subdirectory underneath where the [Module XML File](DevGuideModuleXml.md) is stored._

## Specifics ##

[Module XML Format](DevGuideModuleXml.md)
Modules are defined in XML and placed into your [project's package hierarchy](DevGuideProjectStructure.md).

[Automatic Resource Inclusion](DevGuideAutomaticResourceInjection.md)
Modules can contain references to external JavaScript and CSS files, causing them to be automatically loaded when the module itself is loaded.

[Filtering Public and Source Packages](DevGuidePublicPackageFiltering.md)
Filter files into and out of your public path and source path to avoid publishing files unintentionally.

