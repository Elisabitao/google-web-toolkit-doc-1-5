# Google Web Toolkit: Frequently Asked Questions #
## Getting Started ##
> ### Product Overview ###
    * [What is GWT?](FAQ_WhatIsTheGWT.md)
    * [How does GWT work?](FAQ_HowGWTWorks.md)
    * [Why code in Java and then translate it to JavaScript?](OverviewWhyTranslate.md)
    * [How well does GWT work?](OverviewHowWellDoesGWTWork.md)

> ### Installation and Upgrades ###
    * [What are the system requirements for GWT?](FAQ_GWTSysReqs.md)
    * [How do I install GWT?](FAQ_GWTInstall.md)
    * [Is GWT available in my country? Does it work for my language?](FAQ_GWTCountry.md)
    * [Does GWT cost anything?](FAQ_GWTCost.md)
    * [Why is Google giving GWT away?](FAQ_GWTFree.md)
    * [Will I have to upgrade my application when a new version of GWT is released?](FAQ_NewVersionsOfGWT.md)
    * [How do I get my project to run again after upgrading GWT?](FAQ_WontRunAfterUpgrade.md)

> ### Licensing ###
    * [Can I use GWT to develop commercial/enterprise applications?](FAQ_CommercialAppsWithGWT.md)
    * [Can I redistribute the GWT binaries with my product?](FAQ_Redistribution.md)

> ### Support ###
    * [Does GWT have a blog?](FAQ_GWTBlog.md)
    * [Who do I contact if I have questions?](FAQ_GWTContact.md)
    * [Where should I report bugs?](FAQ_SubmittingBugReports.md)
    * [Where should I submit suggestions to improve GWT?](FAQ_SubmittingSuggestions.md)
    * [Where can I find the GWT source code? Can I submit a patch?](FAQ_SubmitPatch.md)

> ### Browsers and Servers ###
    * [What browsers does GWT support?](FAQ_CurrentlySupportBrowsers.md)
    * [Will my application break when a new browser comes out?](FAQ_NewBrowserSupport.md)
    * [Do I need to run Tomcat on my server to use GWT?](FAQ_TomcatRequiredOnServerForGWT.md)
    * [Can I use GWT with my favorite server-side templating tool?](FAQ_GWTWithServerSideTemplatingTool.md)

## Building the User Interface ##
> ### Layout ###
    * [I'm having trouble with my layout. How do I tell where my panels are?](FAQ_UILayoutDebug.md)
    * [How do I create an application that fills the page vertically when the browser window resizes?](FAQ_UIVerticalFillApp.md)
    * [Why doesn't setting a widget's height as a percentage work?](FAQ_UISetHeightPercentage.md)
    * [Does GWT support standards mode?](FAQ_StandardsModeSupport.md)

> ### Event Handling and Performance ###
    * [How do I display a big list of items in a table with good performance?](FAQ_UILargeTablePerformance.md)
    * [What can I do to make animations appear more smooth and borders load more quickly?](FAQ_UIImagePreload.md)
    * [Why do I see poor performance in Internet Explorer when using a one pixel background image?](FAQ_UIOnePxBackgroundInIE.md)
    * [As the application grows, event listeners seem to fire more slowly.](FAQ_UIUseOneListener.md)
    * [How can I most efficiently handle events from many interior widgets?](FAQ_WidgetEventBubbling.md)

## Writing the Client-side Code in Java ##
> ### Project Architecture ###
    * [What is a GWT module?](FAQ_GWTModuleDefinition.md)

> ### Writing in Java ###
    * [How do I enable assertions?](FAQ_EnableAssertions.md)

> ### JavaScript Native Interface ###
    * [How do I call Java methods from handwritten JavaScript or third party libraries?](FAQ_CallGWTMethodFromHandwrittenJS.md)
    * [Help! I'm having problems with eval() in my JSNI method.](FAQ_EvalJSNIMethod.md)
    * [Why doesn't the bridge call to my JSNI method work in <some\_obj>.onclick?](FAQ_BridgeMethodNotWorkingOnclick.md)
    * [How do I pass a Java method as a callback function?](FAQ_PassGWTMethodAsCallbackFunction.md)

## Debugging in Hosted Mode ##
  * [What are the language differences between web mode and hosted mode?](FAQ_LanguageDifferenceBtwnWebHostedMode.md)
  * [How do I use EJBs in hosted mode?](FAQ_UsingEJBInHostedMode.md)
  * [How do I use my own server in hosted mode instead of GWT's built-in Tomcat instance?](FAQ_HostedModeNoServer.md)

## Compiling Java to JavaScript ##
> ### Output ###
    * [What is deferred binding?](FAQ_DeferredBindingDefinition.md)
    * [What's with all the cache/nocache stuff and weird filenames?](FAQ_GWTApplicationFiles.md)
    * [How do I change the location of my cache/nocache HTML files?](FAQ_ChangeLocationGWTApplicationFiles.md)
    * [Why is my GWT-generated JavaScript gibberish?](FAQ_JavaScriptGibberish.md)
    * [Why don't the project scripts build my Servlets?](FAQ_ProjectScriptBuildServlet.md)
> ### Performance ###
    * [Can I speed up the GWT compiler?](FAQ_CompileOnePermutation.md)

## Communicating with the Server ##
> ### Asynchronous Communication ###
    * [Why doesn't GWT provide a synchronous server connection option?](FAQ_SynchronousServerConnection.md)
> ### Security ###
    * [What is the Same Origin Policy, and how does it affect GWT?](FAQ_SOP.md)

> ### GWT RPC ###
    * [How do I make a call to the server if I am not using GWT RPC?](FAQ_ServerCallWithoutRPC.md)
    * [Does GWT RPC support the use of java.io.Serializable?](FAQ_RPCSerializationSupport.md)

> ### JSON ###
    * [How can I dynamically fetch JSON feeds from other web domains?](FAQ_JSONFeedsFromOtherDomain.md)
    * [How can I coerce a JSON JavaScriptObject into a GWT JSONObject?](FAQ_JSONJSOIntoGWTJSONObject.md)

## Deploying a GWT Application ##
  * [How do I package a GWT application into a WAR file?](FAQ_PackageAppInWARFile.md)
