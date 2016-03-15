# What are the language differences between web mode and hosted mode? #

Java is not the same language as JavaScript, and so web mode will never quite be identical in behavior to hosted mode.  For example, Java's floating point representation has more dynamic range than JavaScript's, and the two languages have different operator precedence.  These problems can cause subtle bugs to appear in web mode that don't appear in hosted mode.  Fortunately those cases are rare.

A [full list of known language-related "gotchas"](DevGuideJavaCompatibility.md) is available in the GWT documentation.
