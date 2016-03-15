# How well does GWT work? #

We're biased, but we think it works pretty darn well. The primary metrics we use to evaluate GWT's effectiveness relative to traditional AJAX development are:

  * **Compiler-generated JavaScript size** - A typical, full-featured GWT application will require the user to download about 100K of cacheable JavaScript, which is in line with most hand-written AJAX applications.

  * **End-user performance** - GWT applications are almost always as fast as hand-written JavaScript. The GWT compiler avoids adding any wrappers around any functionality that is implemented natively in the browser.

  * **Development time** - With so little time spent debugging problems in individual web browsers, you can spend much more of your time on application functionality. Development time efficiency is our favorite part of GWT.

[Try it out for yourself](GettingStarted.md), and [let us know](http://groups.google.com/group/Google-Web-Toolkit) what you think.