# JUnit tests fail on Mac #
Some JUnit tests fail on the Mac platform.

When the Safari/Webkit web browser runs in headless mode it is able to optimize some rendering operations. One result of these optimizations is that the position or size of some elements may be different when running a unit test than when running an actual application. A common symptom is to see the position or size of an widget set to _(0,0)_.

### Workaround ###
To work around this problem, pass the `-notHeadless` argument in the `-Dgwt.args=` system property setting. Note: When using the ant build, this argument is enabled by default as of the GWT 1.5 release.

When `-notHeadless` is specified for running unit tests, the log window and hosted mode browser will pop up on the screen while the test is running. Dismissing or manipulating the elements in these windows may cause the unit tests to fail.
