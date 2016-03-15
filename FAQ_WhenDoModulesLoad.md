# The Bootstrap Sequence #

Consider the following HTML page that loads a GWT module:

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html>
  <body onload='alert("w00t!")'>
    <img src='bigImageZero.jpg'></img>
    <script source='externalScriptZero.js'></script>
    <img src='bigImageOne.jpg'></img>
    <img src='reallyBigImageTwo.jpg'></img>
    <script src='com.example.app.App.nocache.js'></script>
    <script src='externalScriptOne.js'></script>
  </body>
</html>
```

The following principles are needed to understand the sequence of operations that will occur in this page:
  * `<script>` tags always block evaluation of the page until the script is fetched and evaluated.
  * `<img>` tags do **not** block page evaluation.
  * Most browsers will allow a maximum of two simultaneous connections for fetching resources.
  * The `body.onload()` event will only fire once **all** external resources are fetched, including images and frames.
  * The GWT selection script (i.e. `.nocache.js`) will be fetched and evaluated like a normal script tag, but the compiled script will be fetched **asynchronously**.
  * Once the GWT selection script has started, its `onModuleLoad()` can be called at any point after the outer document is done parsing.

Applying these principles to the above example, we obtain the following sequence:
  1. The HTML document is fetched and parsing begins.
  1. Begin fetching `bigImageZero.jpg`.
  1. Begin fetching `externalScriptZero.js`.
  1. `bigImageZero.jpg` completes (let's assume). Parsing is blocked until `externalScriptZero.js` is done fetching and evaluating.
  1. `externalScriptZero.js` completes.
  1. Begin fetching `bigImageOne.jpg` and `reallyBigImageTwo.jpg` simultaneously.
  1. `bigImageOne.jpg` completes (let's assume again). `com.example.app.App.nocache.js` begins fetching and evaluating.
  1. ...nocache.js completes, and the compiled script (...`cache.js`) begins fetching (this is non-blocking).
  1. ...cache.js completes. `onModuleLoad()` is not called yet, as we're still waiting on `externalScriptOne.js` to complete before the document is considered 'ready'.
  1. `externalScriptOne.js` completes. The document is ready, so onModuleLoad() fires.
  1. `reallyBigImageTwo.jpg` completes.
  1. `body.onload()` fires, in this case showing an alert() box.

This is a bit complex, but the point is to show exactly when various resources are fetched, and when `onModuleLoad()` will
be called. The most important things to remember are that
  * You want to put the GWT selection script as early as possible within the body, so that it begins fetching the compiled script before other scripts (because it won't block any other script requests).
  * If you are going to be fetching external images and scripts, you want to manage your two connections carefully.
  * `<img>` tags are not guaranteed to be done loading when `onModuleLoad()` is called.
  * `<script>` tags **are** guaranteed to be done loading when `onModuleLoad()` is called.

## A Note on Multiple GWT Modules and EntryPoints ##

If you have multiple EntryPoints (the interface that defines `onModuleLoad()`) within a module, they will all be called in sequence as soon as that module (and the outer document) is ready.

If you are loading multiple GWT modules within the same page, each module's EntryPoints will be called as soon as both that module and the outer document is ready. Two modules' EntryPoints are not guaranteed to be fired at the same time.
