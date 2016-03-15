# Widgets and Panels #

You construct user interfaces in GWT applications using [widgets](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Widget.html) that are contained within [panels](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Panel.html).  Widgets allow you to interact with the user. Panels control the placement of user interface elements on the page.  Widgets and panels work the same way on all browsers; by using them, you eliminate the need to write specialized code for each browser.

## Widgets ##

Widgets define your applications input and output with the user.  Examples of widgets include the following:

  * [Button](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Button.html) A user clicks the mouse button to activate the button.
> > ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/Button.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/Button.png)
  * [TextBox](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TextBox.html) The application can display text and the user can type in the text box.
> > ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/TextBox.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/TextBox.png)
  * [Tree](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Tree.html) A collapsible hierarchy of widgets.
> > ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/Tree.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/Tree.png)
  * [RichTextArea ](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/RichTextArea.html) A text editor that allows R
> > ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/RichTextArea.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/RichTextArea.png)

You are not limited to the set of widgets provided by GWT. There are a number of ways to [create custom widgets](DevGuideCreatingCustomWidgets.md):

  * You can bundle together existing widgets and create a _composite_ widget.
  * You can write GWT bindings to an existing JavaScript widget.
  * You can create your own widget from scratch using either Java or JavaScript.

You can also use one or more of the many third party widget libraries written for GWT.

## Panels ##

Panels contain widgets and other panels.  They are used to define the [layout](DevGuideUnderstandingLayout.md) of the user interface in the browser.  Examples of panels include the following:

  * [DockPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/DockPanel.html) Arranges widgets and subpanels using directional alignment.
> > ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/DockPanel.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/DockPanel.png)
  * [HorizontalPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HorizontalPanel.html) Arranges widgets and subpanels horizontally.
> > ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/HorizontalPanel.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/HorizontalPanel.png)
  * [TabPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TabPanel.html) Arranges subpanels and widgets under tabs.
> > ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/TabPanel.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/TabPanel.png)
  * [RootPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/RootPanel.html) This panel encapsulates your entire GWT user interface.

## Styles ##

Visual styles are applied to widgets using [Cascading Style Sheets (CSS)](DevGuideStyleSheets.md).  Besides the default browser supplied definitions, each GWT widget and panel has pre-defined style sheet class definitions documented in the class reference documentation.

### See Also ###

  * [Widget Gallery](DevGuideWidgetGallery.md) Diagrams and screen captures of the different GWT UI elements.

  * [Creating Custom Widgets](DevGuideCreatingCustomWidgets.md) Discussion of how to create your own widgets in GWT.

  * [Understanding Layout](DevGuideUnderstandingLayout.md) Examples of how to use panels.
