# Why doesn't setting a widget's height as a percentage work? #
In some cases, the Widget.setHeight() and Widget.setSize() methods do
not seem to work with sizes expressed as percentages.  The GWT widgets do not do any calculations with these values. These methods set the widget's CSS height and/or and width properties and depend on the behavior of the enclosing widget.

In standards mode, if an element (such as a `<`div`>`) is inside a table cell `<`td`>`, you cannot set the `<`div`>` height and width attributes as percentages. They are ignored.  For example, the styles in the `<`div`>` element below is ignored in standards mode:
```
    <td>
      <div style="height:100%;width:100%;>test</div>
    </td>
```

### Workarounds ###
  * Try changing the enclosing outer widget to a different type. For example, VerticalPanel is implemented with an HTML `<`table`>` element and might behave differently than AbsolutePanel which is implemented with an HTML `<`div`>` element.

  * If the `<`DOCTYPE...`>` declaration in the HTML host page is set to standards mode (such as HTML 4.01 Transitional), try removing the `<`DOCTYPE ...`>` declaration or changing it to put the browser rendering engine into quirks mode.

