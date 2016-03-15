<a href='Hidden comment: 
2008-11-27. As of GWT 1.5, this is no longer true. This page is deprecated.
'></a>
# Why does the DOM object expose a non-W3C API? #
Manipulating the contents of the browser's HTML DOM is at the heart of DHTML and AJAX applications.  In most languages and environments, manipulating the DOM follows a convention established by the W3C.  For example, the code to create a new node is usually `document.create('nodename')`. In GWT, however, the equivalent code is `DOM.createElement('nodename')`.  Similarly, the W3C-standard way to append one node to another is with `parent.appendChild(child)`.  In GWT, this is `DOM.appendChild(parent, child)`.

In other words, the DOM API in GWT requires you to call methods on the DOM object itself, instead of directly on the relevant objects.  This might seem odd to some developers, who might be inclined to ask why this is the case.  The reason, in short, is because of [Deferred Binding](FAQ_DeferredBindingDefinition.md).

Every browser has idiosyncrasies, or "quirks".  These browser quirks extend to the DOM APIs exposed by the browsers.  Since each browser's DOM implementation is slightly different, GWT needs to wrap accesses to the browsers' DOMs within a compatibility interface.  That compatibility interface is the GWT DOM object.

The GWT API consolidates all DOM operations into the single DOM class, so that only that one class is affected by the Deferred Binding.  To use the W3C model, all of the related classes would need to have alternate browser-specific implementations. That would make the code harder to maintain and less efficient.

For these reasons, GWT uses a single DOM object instead of a more W3C-standard API to minimize complexity and improve the quality of the software.