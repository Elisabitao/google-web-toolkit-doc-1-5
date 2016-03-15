# JavaScriptObject Overlay Technique (JSO overlay) #

Previous to GWT 1.5, one of the only ways to interact with external JavaScript was to define [JavaScript Native Interface (JSNI)](DevGuideJavaScriptNativeInterface.md) methods. Using JSNI is a powerful and easy way to work in native and with JavaScript code when necessary, however we felt there was more we could add to make the interop story completely seamless.

In GWT 1.5, we've introduced a new way to inter-operate with external JavaScript that allows you to overlay a JavaScriptObject subclass over an existing JavaScript object. This has the advantage of letting you domesticate JavaScript objects from the wild and allowing you to interact with them through a strongly-typed, compiler-optimizable Java object in your GWT code.

You can imagine that this technique opens a wide variety of new things you can do in mixing and matching your GWT code with external JavaScript objects.

A few key benefits we'd like to highlight are:

  * Inter-operating with external JavaScript libraries by overlaying JSO objects on top of library objects you're interested in using.

  * Working with JSON data through an opaque JavaScriptObject subtype.


## Specifics ##
[Overlaying a JavaScript Object using the JSO Overlay Technique](DevGuideJavaScriptObjectOverlayingJavaScriptObject.md)
Create a JavaScriptObject to overlay onto arbitrary JavaScript objects.

[Overlaying JSON data using the JSO Overlay Technique](DevGuideJavaScriptObjectOverlayingJSON.md)
Create a JavaScriptObject to overlay and access JSON data.

[Use Inheritance among JSO subclasses](DevGuideJavaScriptObjectOverlayInheritance.md)
Promote reuse in your code base by introducing inheritance among your JSO subclasses.

[Using the JSO Overlay Technique Securely](DevGuideJavaScriptObjectOverlaySecurityImplications.md)
Design and use your JSO subclasses with security in mind.
