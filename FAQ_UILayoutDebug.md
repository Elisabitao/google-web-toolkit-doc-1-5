# I'm having trouble with my layout.  How do I tell where my panels are? #

When you are working with complex layouts, it is sometimes difficult to understand why your widgets aren't showing up where you think they should.

As an example, suppose you were trying to create a layout with two buttons, one on the left size of the browser, and the other on the right side of the browser.  Here's some code that attempts to do that:

```
    VerticalPanel vertPanel = new VerticalPanel();
    
    DockPanel dockPanel = new DockPanel();
    dockPanel.setWidth("100%");
    dockPanel.add(new Button("leftButton"), DockPanel.WEST);
    dockPanel.add(new Button("rightButton"), DockPanel.EAST);

    vertPanel.add(dockPanel);
    
    RootPanel.get().add(vertPanel);
```

And here is how the screen ends up:

> ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/FAQ_UILayout1.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/FAQ_UILayout1.png)


What went wrong?  We set the Dock Panel to fill up 100% of the space, and then each button to the left or right of the screen.  At this point, it can be helpful to probe and see what the boundaries of your widgets are, because they may not be where you think.

The first technique is to use a DOM inspection tool, such as [Firebug](http://www.getfirebug.com) for the [Firefox](http://www.mozilla.com) browser.  As you hover over elements in the DOM they will highlight in the browser window.

Another technique you can use is to modify your GWT code to turn on borders on your panels (or other widgets.)

```
VerticalPanel vertPanel = new VerticalPanel();
    
    DockPanel dockPanel = new DockPanel();
    dockPanel.setWidth("100%");
    dockPanel.add(new Button("leftButton"), DockPanel.WEST);
    dockPanel.add(new Button("rightButton"), DockPanel.EAST);
    dockPanel.setStylePrimaryName("dockPanel");
    dockPanel.setBorderWidth(5);
    
    vertPanel.add(dockPanel);
    vertPanel.setStylePrimaryName("vertPanel");
    vertPanel.setBorderWidth(5);
    
    RootPanel.get().add(vertPanel);
```

You can also use the associated CSS stylesheet to change the border properties.  In this case, the stylesheet changes the colors of the borders to make them easier to tell apart:

```
$PP_OFF
.dockPanel {
	border-color: orange;
}
.vertPanel {
	border-color: blue;
}
```

Now you can see clearly where the outlines of each panel are:

> ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/FAQ_UILayout2.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/FAQ_UILayout2.png)


In this case, the outermost panel needs to be 100% width as well, so we change the code, hit _Refresh_ on the Hosted Mode browser window:

```
    vertPanel.setWidth("100%");
```

> ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/FAQ_UILayout3.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/FAQ_UILayout3.png)


This is closer to what we want, but it still isn't right.  Now we need to change the cell alignment in the  [DockPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/DockPanel.html) instance:

```
    Button rightButton = new Button("rightButton");
    dockPanel.add(rightButton, DockPanel.EAST);
    dockPanel.setCellHorizontalAlignment(rightButton, HasHorizontalAlignment.ALIGN_RIGHT);
```

> ![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/FAQ_UILayout4.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/FAQ_UILayout4.png)

(Don't forget to turn off these styles when you are finished!)
