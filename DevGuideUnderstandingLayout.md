# Understanding Layout #

Panels in GWT are much like their layout counterparts in other user interface libraries. The main difference lies in the fact that GWT panels use HTML elements such as DIV and TABLE to layout their child widgets.

## RootPanel ##

The first panel you're likely to encounter is the [RootPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/RootPanel.html). This panel is always at the top of the containment hierarchy. The default RootPanel wraps the HTML document's body, and is obtained by calling [RootPanel.get()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/RootPanel.html#get()). If you need to get a root panel wrapping another element in the HTML document, you can do so using [RootPanel.get(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/RootPanel.html#get(java.lang.String)).

## CellPanel ##

[CellPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/CellPanel.html) is the abstract base class for [DockPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/DockPanel.html), [HorizontalPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HorizontalPanel.html), and [VerticalPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/VerticalPanel.html). What these panels all have in common is that they position their child widgets within logical "cells". Thus, a child widget can be aligned within the cell that contains it, using [setCellHorizontalAlignment()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/CellPanel.html#setCellHorizontalAlignment(com.google.gwt.user.client.ui.Widget,%20com.google.gwt.user.client.ui.HasHorizontalAlignment.HorizontalAlignmentConstant)) and [setCellVerticalAlignment()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/CellPanel.html#setCellVerticalAlignment(com.google.gwt.user.client.ui.Widget,%20com.google.gwt.user.client.ui.HasVerticalAlignment.VerticalAlignmentConstant)). CellPanels also allow you to set the size of the cells themselves (relative to the panel as a whole) using [setCellWidth()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/CellPanel.html#setCellWidth(com.google.gwt.user.client.ui.Widget,%20java.lang.String)) and [setCellHeight()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/CellPanel.html#setCellHeight(com.google.gwt.user.client.ui.Widget,%20java.lang.String)).

Below is an example of using a [HorizontalPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HorizontalPanel.html) from the Showcase sample application:

```
    // Create a Horizontal Panel
    HorizontalPanel hPanel = new HorizontalPanel();

    // Leave some room between the widgets
    hPanel.setSpacing(5); 

    // Add some content to the panel
    for (int i = 1; i < 5; i++) {
      hPanel.add(new Button("Button " + i));
    }
```

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/HorizontalPanel_Showcase.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/HorizontalPanel_Showcase.png)

Each new Button widget is added to the right hand side of the panel, creating a horizontal row of widgets.  The `VerticalPanel` works the same way, but adds new widgets to the bottom of the panel and lays them out vertically.

A more powerful layout mechanism is provided by the `DockPanel`.  A `DockPanel` aligns its components using compass directions, where _north_ points up on the screen.  Below is an example of using a `DockPanel` from the Showcase sample application:

```
    DockPanel dock = new DockPanel();

    // Allow 4 pixels of spacing between each cell
    dock.setSpacing(4);

    /* Center each component horizontally within each cell
     * for each component added after this call.
     * A shortcut to calling dock.setCellHorizontalAlignment()
     * for each cell.
     */
    dock.setHorizontalAlignment(DockPanel.ALIGN_CENTER);

    // Add text widgets all around
    dock.add(new HTML("This is the <i>first</i> north component"),
        DockPanel.NORTH);
    dock.add(new HTML("This is the <i>first</i> south component"),
        DockPanel.SOUTH);
    dock.add(new HTML("This is the east component"), DockPanel.EAST);
    dock.add(new HTML("This is the west component"), DockPanel.WEST);
    dock.add(new HTML("This is the <i>second</i> north component"),
        DockPanel.NORTH);
    dock.add(new HTML("This is the <i>second</i> south component"),
        DockPanel.SOUTH);

    // Add scrollable text in the center
    HTML contents = new HTML("This is a <code>ScrollPanel</code> contained at "
        + "the center of a <code>DockPanel</code>.  "
        + "By putting some fairly large contents "
        + "in the middle and setting its size explicitly, it becomes a "
        + "scrollable area within the page, but without requiring the use of "
        + "an IFRAME.<br><br>"
        + "Here's quite a bit more meaningless text that will serve primarily "
        + "to make this thing scroll off the bottom of its visible area.  "
        + "Otherwise, you might have to make it really, really small in order "
        + "to see the nifty scroll bars!");
    ScrollPanel scroller = new ScrollPanel(contents);
    scroller.setSize("400px", "100px");
    dock.add(scroller, DockPanel.CENTER);
```

As you can see in the screenshot below, the order of adding components to the `DocPanel` is important if you add more than one component to the same direction.  The first component added will be closest to the edge.  The next component added will nest toward the inside of the panel.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/DockPanel_Showcase.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/DockPanel_Showcase.png)

## Tab Panel ##

The [TabPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TabPanel.html) displays a row of clickable tabs.  Each tab is associated with a panel which can contain a sub panel or arbitrary HTML which is exposed in a main viewing area when the tab is selected.  The main viewing area is implemented with a [DeckPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/DeckPanel.html).  An example of using a Tab Panel follows:

```
    // Create a tab panel
    TabPanel tabPanel = new TabPanel();

    // Set the width to 400 pixels
    tabPanel.setWidth("400px");

    // Add a home tab
    HTML homeText = new HTML("Click one the tabs to see more content.");
    tabPanel.add(homeText, "Home");

    // Add a tab with an image
    VerticalPanel vPanel = new VerticalPanel();
    vPanel.add(Showcase.images.gwtLogo().createImage());
    tabPanel.add(vPanel, "GWT Logo");

    // Add a third tab
    HTML moreInfo = new HTML("Tabs are highly customizable using CSS");
    tabPanel.add(moreInfo, "More Info");

    // Make the first tab selected and the tab's content visible
    tabPanel.selectTab(0);
```

The screenshot below shows the [TabPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/TabPanel.html) focused on the first tab.  As you can see from the code example, you can add HTML or other panels to be displayed as the content of the tab.  You might also notice that the widget looks different than the default Tab Panel.  The Showcase demo uses [CSS Stylesheets](DevGuideStyleSheets.md) to change the appearance of the panel.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/TabPanel_Showcase.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/TabPanel_Showcase.png)


## Horizontal and Vertical Split panels ##

The [HorizontalSplitPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HorizontalSplitPanel.html) and [VerticalSplitPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/VerticalSplitPanel.html) arrange two widgets in a single row or column and allow the user to interactively change the proportion of the width dedicated to each of the two widgets. Widgets contained within the split panels  will be automatically decorated with scrollbars when necessary.

```
    // Create a Vertical Split Panel
    VerticalSplitPanel vSplit = new VerticalSplitPanel();

    // Set the bounding box in pixels
    vSplit.setSize("500px", "350px");

    /* Set the position of the handle 30% from the top of the
     * panel.
     */
    vSplit.setSplitPosition("30%");

    // Add some content
    String randomText = "This is some text to show how the contents on either "
        + "side of the splitter flow.   "
        + "This is some text to show how the contents on either "
        + "side of the splitter flow.   "
        + "This is some text to show how the contents on either "
        + "side of the splitter flow.   ";
    vSplit.setTopWidget(new HTML(randomText));
    vSplit.setBottomWidget(new HTML(randomText));
```

By manipulating the handle in the middle of the panel, the user can expose more of the upper or lower portion of the panel, as shown in the screenshot below.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/VerticalSplitPanel_Showcase.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/VerticalSplitPanel_Showcase.png)

The `HorizontalSplitPanel` works in the same way, only aligns its children horizontally.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/HorizontalSplitPanel_Showcase.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/HorizontalSplitPanel_Showcase.png)


## Other Panels ##

Other panels include

| [FlowPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/FlowPanel.html) | A panel that formats its child widgets using the default HTML layout behavior. |
|:-------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------|
| [HTMLPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/HTMLPanel.html) | A panel that contains HTML, and which can attach child widgets to identified elements within that HTML. |
| [PopupPanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/PopupPanel.html) | A panel that can "pop up" over other widgets. It overlays the browser's client area (and any previously-created popups).|
| [Grid](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Grid.html)           | A rectangular grid that can contain text, html, or a child widget within its cells. It must be resized explicitly to the desired number of rows and columns. |
| [FlexTable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/FlexTable.html) | A flexible table that creates cells on demand. It can be jagged (that is, each row can contain a different number of cells) and individual cells can be set to span multiple rows or columns. |
| [AbsolutePanel](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/AbsolutePanel.html) | An absolute panel positions all of its children absolutely, allowing them to overlap. |

See the [Widget Gallery](DevGuideWidgetGallery.md) for screenshots or diagrams for these panels.


## Sizes and Measures ##

It is possible to set the size of a widget explicitly using [setWidth()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#setWidth(java.lang.String)), [setHeight()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#setHeight(java.lang.String)), and [setSize()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/UIObject.html#setSize(java.lang.String,%20java.lang.String)). The arguments to these methods are strings, rather than integers, because they accept any valid CSS measurements, such as pixels (128px), centimeters (3cm), and percentage (100%).
