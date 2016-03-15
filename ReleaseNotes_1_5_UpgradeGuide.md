# How to upgrade to GWT 1.5 #

Indeed, GWT 1.5 is the real deal, so it's no surprise that you'll be wanting to make the move to 1.5 and reap the benefits for your GWT applications. However, before you get on your way to making the move, there are a number of [important notes](ReleaseNotes_1_5_ImportantNotes.md) that you should read and understand before upgrading your applications.

Specifically:
  * GWT 1.5 requires Java 1.5 or later: make sure that your application is either already using Java 1.5 code or is ready to make the jump to 1.5 syntax.
  * You'll want to update your code to use Java 1.5 style Annotations and Generics rather than relying on JavaDoc annotations (such as @gwt.typeArgs). If you don't update your code, expect to see lots of warnings during your next GWT compile or hosted mode startup. You can silence the warning messages by adding the "-Dgwt.nowarn.metadata" flag to the process, but that should only be a temporary workaround, and you **should** update your code with standard Java annotations as soon as you can.
  * The `gwt.js` bootstrap file has been deprecated since 1.4 in favor of the `<module>.nocache.js` bootstrap file, and is no longer generated in 1.5. If your application host HTML page bootstraps using the `gwt.js` file, now is the time to make the change to the new `<module>.nocache.js` file.
  * Make sure your application isn't sensitive to any of these [breaking changes](ReleaseNotes_1_5_BreakingChanges.md).
  * If you're making the upgrade from 1.3 to 1.5, be sure to check out the [breaking changes](http://code.google.com/webtoolkit/releases/release-notes-1.4.62.html) introduced from 1.3 to 1.4 as these still apply in 1.5 (ignoring notes about @gwt.typeArgs JavaDoc annotations as these must be replaced by real Java 1.5 annotations).


---


_If your application checks out on all of the above points and you're ready to ride 1.5, follow the instructions below to upgrade_:

  1. [Download GWT 1.5](http://code.google.com/webtoolkit) for your platform at the link below and unpack it to the directory of your choice:
  1. **Update your GWT project build path** to use the latest GWT JARs. This includes gwt-user.jar, gwt-dev-

&lt;platform&gt;

.jar and gwt-servlet.jar if your application uses GWT-RPC.
  1. **Update any run configurations or application compile and shell scripts** to include the latest JARs in the classpath (same JARs as mentioned in step 2).
  1. **Run a GWT compilation** over your project to generate the latest GWT application files for your project.
  1. **Deploy the latest GWT application files** to your web server.

Any problems while upgrading? As always, let us know on the [GWT Developer Forum](http://groups.google.com/group/Google-Web-Toolkit) and our great community or GWT team members will get back to you to make sure the upgrade process detailed in this guide is accurate and works for our users.