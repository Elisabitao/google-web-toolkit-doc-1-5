# Setting up and tearing down JUnit test cases that use GWT code #

When using a test method in a JUnit [TestCase](http://junit.sourceforge.net/javadoc/junit/framework/TestCase.html), any objects your test creates and leaves a reference to will remain active.  This could interfere with future test methods.  In the GWT 1.5 release, you can override two new methods to prepare for and/or clean up after each test method.

  * [gwtSetUp()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html#gwtSetUp()) runs before each test method in a test case.
  * [gwtTearDown()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html#gwtTearDown()) runs after each test method in a test case.

In GWT versions prior to GWT 1.5, you can override the standard JUnit [setUp()](http://junit.sourceforge.net/javadoc/junit/framework/TestCase.html#setUp()) and [tearDown()](http://junit.sourceforge.net/javadoc/junit/framework/TestCase.html#tearDown()) methods.  However, you are not allowed to use [JSNI methods](DevGuideJavaScriptNativeInterface.md) or code that depends on [deferred binding](DevGuideDeferredBinding.md) (which includes almost all of the UI library).

The following example shows how to defensively cleanup the DOM before the next test run using [gwtSetUp()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/junit/client/GWTTestCase.html#gwtSetUp()).  It skips over `<iframe>` and `<script>` tags so that the GWT test infrastructure is not accidentally removed.

```
  import com.google.gwt.junit.client.GWTTestCase;
  import com.google.gwt.user.client.DOM;
  import com.google.gwt.user.client.Element;

  private static native String getNodeName(Element elem) /*-{
     return (elem.nodeName || "").toLowerCase();
  }-*/;

  /**
   * Removes all elements in the body, except scripts and iframes.
   */
  public void gwtSetUp () {
    Element bodyElem = RootPanel.getBodyElement();

    List<Element> toRemove = new ArrayList<Element>();
    for (int i = 0, n = DOM.getChildCount(bodyElem); i < n; ++i) {
      Element elem = DOM.getChild(bodyElem, i);
      String nodeName = getNodeName(elem);
      if (!"script".equals(nodeName) && !"iframe".equals(nodeName)) {
        toRemove.add(elem);
      }
    }

    for (int i = 0, n = toRemove.size(); i < n; ++i) {
      DOM.removeChild(bodyElem, toRemove.get(i));
    }
  }
```
