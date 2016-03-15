# Running in Web Mode #
After you have your application working well in _hosted_ mode, you will want to try out your application in your target web browsers; that is, you want to run it in _web_ mode. This is as easy as clicking the Compile/Browse button in the toolbar of the Hosted Mode browser.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/WebMode1.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/WebMode1.png)

After you click the Compile/Browse button, the Hosted Mode Shell pauses while the GWT compiler translates your Java source code (plus the source code of the GWT library and any third party libraries) into optimized JavaScript. If the compilation runs into errors, they will be displayed in the Development Shell window. After the compilation completes successfully, a new browser window opens displaying your HTML host page and your application. (Or, an existing browser session might display your application in a new tab.)

Running your application in web mode allows you to test your application as it is deployed. If you have a servlet component and have specified the `<`servlet`>` tag in your module XML file, your GWT RPC calls will also be served to the browser. You can also take a different browser or a browser running on another machine and point it at the same URL (substitute the hostname or IP address of your workstation for `localhost` in the URL.)

Running in web mode is a good way to test:

  * **The performance of your application**
> Hosted mode uses a series of special tricks to run Java code in Java, and native JavaScript code in the Hosted Mode shell's JavaScript engine. When switching back and forth, there can be overhead that makes Hosted mode run more sluggishly than the compiled application running in a browser.

> If your application displays lots of data or has a large number of widgets, you will want to confirm that performance will be acceptable when the application is finally deployed.

  * **How your application looks on different browsers**
> Because GWT widgets use a browser's native DOM components, the look and feel of your application might change from browser to browser. More importantly, if you are using a style sheet, you will want to inspect your application carefully on each browser.

  * **How your application logic performs on different browsers**
> GWT is designed to provide cross-browser support so that the average GWT developer does not need to worry about cross-browser support. But if you are a widget author or if you are using a third party JavaScript library, you will need to confirm that these components are working correctly on each target browser you plan to support.
