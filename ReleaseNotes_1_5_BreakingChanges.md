# Breaking changes in GWT 1.5 #

  * **UI Library changes**:
    * [UIObject.setElement](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#setElement(com.google.gwt.user.client.Element)) has been changed back to its pre-GWT 1.4 behavior - it is now callable only once. For those widgets that need to replace their DOM element once it has been set, the package-protected method replaceElement(elem) in [UIObject](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html) can be used.
    * [Element.getAttribute(name)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/dom/client/Element.html#getAttribute(java.lang.String)) now returns an empty string instead of null for attributes that do not exist.
    * Composite.getElement() has been removed.
    * [Composite.onBrowserEvent()](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Composite.html#onBrowserEvent(com.google.gwt.user.client.Event)) is now invoked properly when sinkEvents is called.
    * [HTMLPanel.add(Widget, id)](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLPanel.html#add(com.google.gwt.user.client.ui.Widget,%20java.lang.String)) is a much faster operation, because it now relies on calling the native document.getElementById(id) method.
    * [HTMLPanel](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLPanel.html) has two new methods - [getElementById(id)](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLPanel.html#getElementById(java.lang.String)), and [addAndReplaceElement(widget, id)](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLPanel.html#addAndReplaceElement(com.google.gwt.user.client.ui.Widget,%20java.lang.String)).
    * GWT Widgets now sink their events lazily: widgets no longer routinely sink their events eagerly. Instead, the event is sunk the first time a listener is added to the widget. So subclasses which relied on eagerly sunk events will now have to manually sink the events they depend upon.
    * The [Tree](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Tree.html) widget no longer uses leaf images by default. Must use the [Tree(TreeImages)](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Tree.html#Tree(com.google.gwt.user.client.ui.TreeImages)) constructor to get leaf images.
    * RichText.gwt.xml no longer inherits the I18N module.
    * [PasswordTextBox](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/PasswordTextBox.html) now inherits from [TextBox](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TextBox.html) rather than [TextBoxBase](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TextBoxBase.html).

  * **History changes**:
    * History listeners are now called synchronously on History.newItem(...) rather than asynchronously after a delay.
    * History.onHistoryChanged(...) is now deprecated, and has been replaced by History.fireCurrentHistoryState(...) for the usual case of triggering the initial history state in onModuleLoad.

  * **GWT Bootstrap File changes**:
    * In GWT 1.3, the bootstrap script that needed to be included in a host HTML page to load up your GWT application was the generated gwt.js file.  In GWT 1.4, a new bootstrapping sequence was introduced where a module.nocache.js was the new bootstrap file, and gwt.js, while generated, was deprecated. In GWT 1.5, the compiler no longer generates the gwt.js file and strictly supports the 1.4 bootstrap model.
    * The cross-site bootstrap (module-xs.nocache.js file) is no longer produced by default; this has been replaced with the "xs" linker. In order to generate cross-site output, you must add the <add linker name="xs" /> tag in your module XML file to invoke the xs linker.

  * **Compiler, Hosted Mode and Generator changes**:
    * Generators cannot [tryCreateResource()](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/ext/GeneratorContext.html#tryCreateResource(com.google.gwt.core.ext.TreeLogger,%20java.lang.String)) if a resource by that name is on your project's public path.
    * [TreeLogger](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/ext/TreeLogger.html) API changes.
    * Standard Java 1.5 Annotations replace JavaDoc metadata (such as `@gwt.typeArgs`), which are now deprecated and will be removed in an upcoming release.

  * **JSNI, JSON and JSO changes**:
    * JSNI marshalling changes: marshalling is generally based on the runtime type of object rather than the declared type; if the runtime type is incompatible, an exception will occur in hosted mode.
    * Strings cannot be marshalled as JSOs anymore
    * Java longs are now emulated in Javascript. Direct access to longs from JSNI code is now forbidden.
    * [JSONObject.get()](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONObject.html#get(java.lang.String)) no longer checks hasOwnProperty.

  * **JUnit and Benchmark changes**:
    * Benchmarking subsystem moved packages from com.google.gwt.junit to com.google.gwt.benchmarks.

  * **I18N Messages properties files**:
    * In earlier releases of GWT, unpaired single quotes could sometimes be used in properties files as literal characters.  However, the [Messages](http://google-web-toolkit-doc-1-5.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html) interface uses [MessageFormat](http://java.sun.com/j2se/1.5.0/docs/api/java/text/MessageFormat.html)-style messages, using single quotes to quote other characters.  For example `The string '{0}' is used for parameter replacement` -- single quotes intended to be part of the string should be doubled.  The old behavior was in violation of the spec and led to inconsistent results depending on where the quote was used.