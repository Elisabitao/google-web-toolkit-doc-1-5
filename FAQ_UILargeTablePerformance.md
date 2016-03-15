# How do I display a big list of items in a table with good performance? #
Here are a few pointers for addressing the need to display lots of data through a table widget efficiently.

## Fixed width tables ##
Performance wise, you are better off using a fixed width table than one that dynamically resizes.
<a href='Hidden comment: 
TBD - Emily what did you mean here?  (CSS width in pixels or em, not %?)
'></a>
## RPC and other network transaction considerations ##
Most applications are loading the data for their table over the network. Try to reduce the number of round trips your application has to make to populate the table. Two main strategies come to mind:
  * Fetch multiple rows in the same request.
  * Combine activity from the different subsystems of the app into a single RPC call. From the combined data structure so you can fetch data relevant to different parts of the application in the same RPC call and reduce user wait time.

## Testing for Performance ##
If you have a large table, make sure you run a test with the largest number of rows you want to support. On some browsers the GWT team has found that table performance is non-linear with the number of rows.

## Try using PagingScrollTable ##
If you find yourself with a table performance issue, the best way to address it might be not display most of your data. A table implementation called the [PagingScrollTable](http://code.google.com/p/google-web-toolkit-incubator/wiki/PagingScrollTable) can help you display data a page at a time instead of having to wait for the entire dataset to download to the browser. This breed of table is very common in web apps and won't be foreign to users. As a rule of thumb, consider using PagingScrollTable if you have more than 50 rows of data to display.

Note that the PagingScrollTable is a part of the [GWT Incubator](http://code.google.com/p/google-web-toolkit-incubator/) project which you will need to download separately from the core GWT distribution.

If you can use the PagingScrollTable, then consider the [CachedTableModel](http://code.google.com/p/google-web-toolkit-incubator/wiki/PagingScrollTable) (also a part of the [GWT Incubator](http://code.google.com/p/google-web-toolkit-incubator/)) you can use to automatically prefetch rows, such as the next page. This can increase the perceived speed considerably.

## Try using the BulkTableRenderer and PreloadedTable classes ##
A [TableBulkRenderer](http://code.google.com/p/google-web-toolkit-incubator/wiki/BulkTableRenderers) will render all rows in a table at once. BulkTableRenderer has derived types for different purposes. As long as your table contents are not widgets, you provide a table model and the BulkRenderer creates the entire table rendered as a single HTML string and set with [setInnerHtml()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/dom/client/Element.html#setInnerHTML(java.lang.String)) which can be 2-10x faster.

Note that after being loaded, widgets, cell spans, row spans etc. may be added to the table, but there will be no speed advantage for them.
