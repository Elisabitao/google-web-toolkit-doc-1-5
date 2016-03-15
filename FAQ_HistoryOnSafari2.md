# GWT History feature is broken in Safari 2.0 #
  * Q: The GWT History feature is great, but it has been driving me bananas on Safari 2.0. What's the history between GWT and Safari 2.0?
  * A: Unfortunately, the GWT History system does not play well with Safari 2.0.

A number of strange bugs in Safari 2, which are particularly difficult to work around, make it hard to use the same tricks GWT uses for history support in IE and Firefox. Some messy solutions do exist but these fail in a number of significant edge cases and are not likely to be good long-term solutions.

The current implementation provides fall-back functionality for Safari 2 so that the library works but no history entries are generated.

Although the current GWT solution for history support doesn't completely work on Safari 3 beta, it has been tested on the latest nightly WebKit builds and is looking good.

So, hold on to your hats! History support for Safari will work properly when Safari 3 is officially released.
