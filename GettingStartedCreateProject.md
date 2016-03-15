# Create a GWT project #

Let's start by creating a directory to place our GWT project in. Create a new directory named `StockWatcher` under the main GWT directory (e.g., `gwt-windows-1.5.3`).  To actually create our project we'll use some command-line utilities that ship with GWT. These utilities take care of generating the project subdirectories and files we need to get started.

## Create an Eclipse project with projectCreator ##

If you're using Eclipse, the first utility you'll run is [projectCreator](DevGuideProjectCreator.md) (_if you're not using Eclipse, skip ahead to the section on applicationCreator_). The `projectCreator` utility will generate a shell Eclipse project for our application. Open a command shell and browse to the `StockWatcher` directory you created. Run the command:

```
$PP_OFF
projectCreator -eclipse StockWatcher -out StockWatcher
```

After the `projectCreator` utility runs, you should see that it has generated a new directory called `StockWatcher` containing a couple of subdirectories (`src` and `test` for storing the project source code and unit tests) and a pair of Eclipse files (`.project` and `.classpath`).

## Create a GWT project with applicationCreator ##

Next we're going to use the [applicationCreator](DevGuideApplicationCreator.md) utility to generate a minimal but functional GWT project. We'll need to pass it the fully-qualified name of our application's main entry point class. Based on the [recommended GWT project structure](DevGuideProjectStructure.md), this class should always be in a subpackage `client`. For our StockWatcher application we'll name our main class `com.google.gwt.sample.stockwatcher.client.StockWatcher`.

The `applicationCreator` utility can also generate Eclipse launch config files for easy hosted mode debugging. Just specify the `-eclipse` flag followed by the name of your Eclipse project.

In a command shell, browse to the `StockWatcher` directory. If you're using Eclipse, run the command:

```
$PP_OFF
applicationCreator -eclipse StockWatcher -out StockWatcher com.google.gwt.sample.stockwatcher.client.StockWatcher
```

If you're using another IDE, omit the `-eclipse` flag and project name:

```
$PP_OFF
applicationCreator -out StockWatcher com.google.gwt.sample.stockwatcher.client.StockWatcher
```

Now take a look inside the `StockWatcher` directory. You'll see that the `applicationCreator` has generated two scripts for us named `StockWatcher-compile` and `StockWatcher-shell`. These scripts will help us prepare StockWatcher for [deployment](GettingStartedWebMode.md), or run the application in [hosted mode](DevGuideHostedMode.md), respectively.

## Test the generated project ##

Let's go ahead and try running our nascent StockWatcher application in [hosted mode](DevGuideHostedMode.md) right now by running the `StockWatcher-shell` script.  When you do, you should see two new windows appear. The first is the Development Shell, which contains a log viewer to display status and error messages. The second is the [hosted mode web browser](DevGuideHostedMode.md).  This is where your GWT application lives while running in hosted mode. You can interact with the application in hosted mode just as you would when it's eventually deployed.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherHostedMode.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherHostedMode.png)

So what is hosted mode, exactly?  In a nutshell, when applications run in hosted mode they are not translated to JavaScript. Instead, the Java Virtual Machine (JVM) executes the application's code as compiled Java bytecode, using GWT plumbing to automate an embedded browser window. This makes it possible to interactively debug GWT applications from their original source code within your IDE just as you would with any other Java application. By remaining in this traditional "code-test-debug" cycle, hosted mode is by far the most productive way to develop your application quickly.

The [applicationCreator](DevGuideApplicationCreator.md) can automatically generate launch configurations for debugging our GWT applications in hosted mode with Eclipse. To use them, we'll need to first import the StockWatcher project into Eclipse.

## Import the project into Eclipse ##

To open the StockWatcher project in Eclipse, launch Eclipse and click on the _File -> Import..._ menu item. Choose "Existing Projects into Workspace" in the first screen of the wizard.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/ImportProject1.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/ImportProject1.png)

Next, enter the directory in which you generated the `.project` file.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/ImportProject2.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/ImportProject2.png)

When you click _Finish_, you should see your GWT project loaded into your Eclipse workspace.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherEclipseProject.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherEclipseProject.png)

To run the project in hosted mode from Eclipse, select the project name in Package Explorer and then click the green _Run_ button in the toolbar (or select the _Run -> Run_ menu item).

## Inside a GWT project ##

Now that we have our initial version of StockWatcher up and running, let's talk about what makes up a GWT project.  Going back to the `StockWatcher` directory we created, note that `applicationCreator` has created a set of nested directories under the `src` subdirectory corresponding to the packages containing our GWT application.

### Modules ###

Our application's root package is in `src/com/google/gwt/sample/stockwatcher`, so let's start there.  You should find one file, named `StockWatcher.gwt.xml`:

```
<module>

      <!-- Inherit the core Web Toolkit stuff.                        -->
      <inherits name='com.google.gwt.user.User'/>
	
      <!-- Inherit the default GWT style sheet.  You can change       -->
      <!-- the theme of your GWT application by uncommenting          -->
      <!-- any one of the following lines.                            -->
      <inherits name='com.google.gwt.user.theme.standard.Standard'/>
      <!-- <inherits name="com.google.gwt.user.theme.chrome.Chrome"/> -->
      <!-- <inherits name="com.google.gwt.user.theme.dark.Dark"/>     -->

      <!-- Other module inherits                                      -->


      <!-- Specify the app entry point class.                         -->
      <entry-point class='com.google.gwt.sample.stockwatcher.client.StockWatcher'/>
    
      <!-- Specify the application specific style sheet.              -->
      <stylesheet src='StockWatcher.css' />
	
</module>
```

This is a GWT [module](DevGuideModules.md). The module contains the [configuration settings](DevGuideModuleXml.md) (in the form of an XML file) for a particular GWT application or library. Our newly-created module contains two `<inherits>` tags, an `<entry-point>` tag, and a `stylesheet` tag. The `<inherits>` tags tells GWT which additional modules your application depends on. This may be one or more of the [built-in modules](FAQ_GWTModuleInheritance.md) or other GWT modules you developed. We only need to inherit one built-in GWT module to create a GWT application: `com.google.gwt.user.User`, which contains the core GWT libraries.  The default application also inherits `com.google.get.user.theme.standard.Standard`, which adds default styles to the Widgets used in the application (see [Add CSS styling](GettingStartedStyle.md) for more info).

The `<entry-point>` tag tells GWT which Java class is your application's startup class. This class must implement the [EntryPoint](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/EntryPoint.html) interface, which defines one method: [onModuleLoad()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/EntryPoint.html#onModuleLoad()). The onModuleLoad() method is the place to do any initialization your application needs to perform, such as creating the UI elements and registering event handlers. Our application's main class is `com.google.gwt.sample.stockwatcher.client.StockWatcher`.

The `<stylesheet>` tag tells GWT to include a style sheet names `StockWatcher.css`, which is located in the public directory of the application.

### Java source code ###

If you browse to the directory `src/com/google/gwt/sample/stockwatcher/client` you'll find the source file for that class in `StockWatcher.java`. Right now it contains very simple "Hello, World" style functionality, which we'll soon replace with our StockWatcher code. In general, the `client` directory will contain all of our client-side source files and any subpackages.

### Static resources ###

Back to the directory tree, take a look inside `src/com/google/gwt/sample/stockwatcher/public`. The `public` directory is a repository for any static resources that we need to include with our GWT application. These could be CSS files or images, for example. When we're ready to deploy our application, the GWT compiler will automatically copy all of the resources in `public` to the output location along with the JavaScript files it generates. Right now our application has only two files in the `public` directory: `StockWatcher.html` and `StockWatcher.css`.

#### HTML host page ####

`StockWatcher.html` is our application's [host page](DevGuideHostPage.md). A host page is the container for a GWT application. It's a regular HTML file that contains a `<script>` tag pointing to your application's startup script. This script is named after the fully-qualified module name, followed by `.nocache.js`:

```
<script language="javascript" src="com.google.gwt.sample.stockwatcher.StockWatcher.nocache.js"></script>
```

We'll revisit our `StockWatcher.html` host page as we build the sample. For the time being, though, just keep in mind that it functions as our GWT application's container and that it _must_ reference our `.nocache.js` startup script.

#### CSS style sheet ####

Our application also includes a CSS style sheet that contains some basic style definitions used in the application.  You can add or remove style definitions just like you would in a traditional web application.

In addition to the style sheet created by application creator, you can use any one of the visual themes that GWT provides.  The visual themes add default styles to all of your GWT widgets, giving your application a more polished look.  See the tutorial on [Adding CSS styling](GettingStartedStyle.md) for more information.

#### Adding GWT to an existing web page ####

One additional fact you should know about host pages is that they don't have to be new HTML files created from scratch.  The flexible architecture of GWT makes it a snap to integrate GWT with existing web pages, including those that use a server-side template framework.

Let's say, for example, that your existing website was built with [PHP](http://www.php.net/). You'd like to add some client-side behavior to one of your pages using GWT. No problem.  Just add a `<script>` tag pointing to the GWT startup script inside your PHP page's `<head>` tag:

```
<html>
  <head>
    <title>Wrapper PHP for StockWatcher</title>
    <script language='javascript' src='com.google.gwt.sample.stockwatcher.StockWatcher.nocache.js'></script>
  </head>
  <body>
  <?php
  echo 'PHP version: ' . phpversion();
  ?>
  </body>
</html>
```

By adding that one line, this PHP page has become a host page for GWT. You can add GWT widgets and client-side behaviors to it just as you can with static `.html` host pages. GWT does not need to explicitly _support_ PHP for this to work. In fact, since GWT runs on the client-side (unlike server-side  PHP scripts), it is not even _aware_ of the fact that the host page was dynamically generated with PHP. To GWT, it's just another host page. This means that GWT can seamlessly work with whatever server-side framework you happen to use, be it [PHP](http://www.php.net/), [Ruby on Rails](http://www.rubyonrails.org/), or even [ASP.NET](http://www.asp.net/).