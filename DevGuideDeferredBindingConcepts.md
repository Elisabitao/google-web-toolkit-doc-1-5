# Deferred Binding Benefits #

Deferred Binding is a technique used by the GWT compiler to create and select a specific implementation of a class based on a set of parameters. In essence, deferred binding is the Google Web Toolkit answer to Java reflection.  It allows the GWT developer to produce several variations of their applications custom to each browser environment and have only one of them actually downloaded and executed in the browser.

Deferred binding has several benefits:

  * Reduces the size of the generated JavaScript code that a client will need to download by only including the code needed to run a particular browser/locale instance (used by the [Internationalization module](DevGuideInternationalization.md))
  * Saves development time by automatically generating code to implement an interface or create a proxy class (used by the [GWT RPC module](DevGuideRemoteProcedureCalls.md))
  * Since the implementations are pre-bound at compile time, there is no run-time penalty to look up an implementation in a data structure as with dynamic binding or using virtual functions.

> _Tip: Check out the FAQ on [deferred binding](FAQ_DeferredBindingDefinition.md) that discusses the differences between static, dynamic and deferred binding.
>_

Some parts of the toolkit make implicit use of deferred binding, that is, they use the technique as a part of their implementation, but it is not visible to the user of the API.  For example, many [widgets and panels](DevGuideWidgetsAndPanels.md) as well as the [DOM](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/DOM.html) class use this technique to implement browser specific logic.  Other GWT features require the API user to explicity invoke deferred binding by designing classes that follow specific rules and instantiating instances of the classes with [GWT.create(Class)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class)), including [GWT RPC](DevGuideRemoteProcedureCalls.md) and [I18N](DevGuideInternationalization.md).

As a user of the Google Web Toolkit, you may never need to create a new interface that uses deferred binding.  If you follow the instructions in the guide for creating internationalized applications or GWT RPC calls you will be using deferred binding, but you will not have to actually write any browser dependent or locale dependent code.

The rest of the deferred binding section describes how to create new rules and classes using deferred binding.  If you are new to the toolkit or only intend to use pre-packaged widgets, you will probably want to skip on to the next topic.  If you are interested in programming entirely new widgets from the ground up or other functionality that requires cross-browser dependent code, the next sections should be of interest.
