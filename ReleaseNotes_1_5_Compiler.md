# Compiler, Hosted Mode & Linkers #

### Java 5 language support! ###
GWT 1.5 supports the Java 5 syntax, including generics, enumerated types, annotations, enhanced for-loop syntax, and static imports.

Taking full advantage of generics and annotations will require some changes in your code (as described in [the important notes](ReleaseNotes_1_5_ImportantNotes.md)), but you'll be glad you did.

### JavaScriptObject subclassing ###
It is now possible to subclass the GWT [JavaScriptObject](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JavaScriptObject.html) (JSO) class to create Java "class overlays" onto arbitrary JavaScript objects. Thus, you can get the benefits of modeling JS objects as proper Java types (e.g. code completion, refactoring, inlining) without any additional memory or speed overhead. This capability makes it possible to use JSON structures optimally and serves as the basis for the all-new GWT DOM package.

### Method inlining ###
The compiler now selectively inlines the bodies of both Java methods and [JSNI](DevGuideJavaScriptNativeInterface.md) methods. The net effect is that the contents of methods may be hoisted into their original call sites, completely removing function call overhead for trivial methods. This optimization is performed recursively, so many levels of indirection can be automatically removed, often reducing code size and always improving performance.

### Assertion support ###
For debugging purposes, the GWT compiler can be made to generate code that checks `assert` statements in web mode by passing "-ea" as an argument to the compiler. Note this is not the same as specifying "-ea" as a JVM flag, which causes the assertions in the GWT tools themselves to be checked.  By default, assertions are removed for production.


### Stable identity for JSOs ###
All objects of types that extend JavaScriptObject now have a stable identity in hosted mode, including `Element` objects from the new [DOM package](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/dom/client/package-summary.html). Thus, you can safely use the normal Java identity comparisons (`==`) on Elements.

`DOM.compare()` is deprecated; use `==` instead to compare the identity of elements, etc.

### Equivalence of null and undefined ###
It's now okay to return `undefined` values from JSNI methods; such values are treated as equivalent to `null` in Java code. This keeps JSNI methods simpler, smaller, faster, and easier to inline.

### Long emulation ###
The Java `long` type now works correctly, giving you the full proper range of a 64-bit integer. Previously, due to JavaScript's lack of true 64-bit integral types, `long` was mapped to the integer range of `double`; the GWT compiler now compensates by generating smarter, more correct (albeit slower) code. See the section on [Java language compatibility](DevGuideJavaCompatibility.md) for more details.

### Introducing the new Linker subsystem ###

Developers now have full control over the packaging and bootstrap sequence of a GWT module thanks to the new Linker subsystem. The Linker subsystem is designed to be an extensible subsystem to allow developers to generate content based on their specific application needs. GWT provides two linkers right out of the box for standard and cross-site output, generated in the "std" and "xs" directories respectively.
