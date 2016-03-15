# ImageBundle images not showing in Internet Explorer #
ImageBundle images show up in Firefox, Opera and Safari, but aren't showing up in Internet Explorer.

## Security ##
The first thing to verify is whether or not your web application uses HTTPS, or if any of the images included in the generated image bundle file are under a security constraint.

If that is the case, you may be running into a problem rooted in your web server setting certain response headers (such `Pragma: No-cache, Expires: Thu, 1 Jan 1970 00:00:00,` ...) and Internet Explorer respecting those headers when when using the HTTPS protocol.

### Workaround ###
You can read more details about this security issue and how to work around it in the API Reference, [Image Bundles and the HTTPS Protocol](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/ImageBundle.html).

## Size ##
On the other hand, if you're not using HTTPS but are still running into this problem, check out the size of your image bundle file.

A number of developers have run into the problem of Internet Explorer consuming large amounts of memory when extracting images out of a larger image bundle, and subsequently failing to properly display the images.

### Workaround ###
You might want to consider splitting up one large image bundle into several smaller bundles. This technique has worked for most developers who have experienced this problem.
