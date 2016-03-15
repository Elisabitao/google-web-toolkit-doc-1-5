# Deferred Binding Using Replacement #

The first type of deferred binding uses _replacement_.  Replacement means overriding the implementation of one java class with another that is determined at compile time.  For example, this technique is used to conditionalize the implementation of some widgets, such as the [PopupPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/PopupPanel.html).  The use of `<inherits>` for the `PopupPanel` class is shown in the previous section describing the [Deferred Binding Rules](DevGuideDeferredBindingUsing.md).  The actual replacement rules are specified in `Popup.gwt.xml`, as shown below:

```
<module>

  <!--  ... other configuration omitted ... -->

  <!-- Fall through to this rule is the browser isn't IE or Mozilla -->
  <replace-with class="com.google.gwt.user.client.ui.impl.PopupImpl">
    <when-type-is class="com.google.gwt.user.client.ui.impl.PopupImpl"/>
  </replace-with>

  <!-- Mozilla needs a different implementation due to issue #410 -->
  <replace-with class="com.google.gwt.user.client.ui.impl.PopupImplMozilla">
    <when-type-is class="com.google.gwt.user.client.ui.impl.PopupImpl" />
    <any>
      <when-property-is name="user.agent" value="gecko"/>
      <when-property-is name="user.agent" value="gecko1_8" />
    </any>
  </replace-with>
  
  <!-- IE has a completely different popup implementation -->
  <replace-with class="com.google.gwt.user.client.ui.impl.PopupImplIE6">
    <when-type-is class="com.google.gwt.user.client.ui.impl.PopupImpl"/>
    <when-property-is name="user.agent" value="ie6" />
  </replace-with>
</module>
```

These directives tell the GWT compiler to swap out the `PoupImpl` class code with different class implementations according to the  the `user.agent` property.  The `Popup.gwt.xml` file specifies a default implementation for the `PopupImpl` class, an overide for the Mozilla browser (`PopupImplMozilla` is substituted for `PopupImpl`), and an override for Internet Explorer version 6 (`PopupImplIE6` is substituted for `PopupImpl`).  Note that `PopupImpl` class or its derived classes cannot be instantiated directly.  Instead, the `PopupPanel` class is used and the [GWT.create(Class)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class)) technique is used under the hood to instruct the compiler to use deferred binding.

# Example Class Hierarchy using Replacement #

To see how this is used when designing a widget, we will examine the case of the `PopupPanel` widget further.  The `PopupPanel` class implements the user visible API and contains logic that is common to all browsers.  It also instantiates the proper implementation specific logic using the  [GWT.create(Class)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class)) as follows:

```
  private static final PopupImpl impl = GWT.create(PopupImpl.class);
```

> _Tip: On GWT versions prior to 1.5, a `(PopupImpl)` cast is needed for the return value from `GWT.create()`
>_

The two classes PopupImplMozilla and PopupImplIE6 extend the PopupImpl class and override some `PopupImpl`'s methods to implement browser specific behavior.

Then, when the `PopupPanel` class needs to switch to some browser dependent code, it accesses a member function inside the `PopupImpl` class:

```
  public void setVisible(boolean visible) {
    // ... common code for all implementations of PopupPanel ...

    // If the PopupImpl creates an iframe shim, it's also necessary to hide it
    // as well.
    impl.setVisible(getElement(), visible);
  }
```

The default implementation of `PopupImpl.setVisible()` is empty, but `PopupImplIE6` has some special logic implemented as a [JSNI](DevGuideJavaScriptNativeInterface.md) method:

```
  public native void setVisible(Element popup, boolean visible) /*-{
    if (popup.__frame) {
      popup.__frame.style.visibility = visible ? 'visible' : 'hidden';
    }
  }-*/;{
```

After the GWT compiler runs, it prunes out any unused code.  If your application references the `PopupPanel` class, the compiler will create a separate JavaScript output file for each browser, each containing only one of the implementations: `PopupImpl`, `PopupImplIE6` or `PopupImplMozilla`.  This means that each browser only downloads the implementation it needs, thus reducing the size of the output JavaScript code and minimizing the time needed to download your application from the server.