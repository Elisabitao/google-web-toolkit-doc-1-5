# Why is my GWT-generated JavaScript gibberish? #

By default, GWT obfuscates the JavaScript it produces.  This is partly done to protect the intellectual property of the application you develop, but also because obfuscation reduces the size of the generated JavaScript files, making them faster to download and parse.

If you prefer not to have GWT obfuscate its output, then you can use the `-style` flag on the GWT Compiler.  This flag has one of three possible values:

  * `OBF` (for obfuscated), the default
  * `PRETTY`, which makes the output readable to a human
  * `DETAILED`, which improves on `PRETTY` with even more detail (such as very verbose variable names)


If you are curious about what GWT's generated JavaScript is doing, then you can use `-style PRETTY`.  On the rare occasion where you are debugging GWT's output, then `-style DETAILED` might be helpful.