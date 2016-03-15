# What can I do to make images and borders appear to load more quickly the first time they are used? #

The first time your application accesses an image, you may notice that it may only partially load, or load more slowly than it displays on subsequent invocations.  Since first impressions are important, you may want to try this trick.

The underlying issue is that the images needed for the background, border, or animation are fetched from the server on demand.  A solution is to pre-fetch the images so that they are sitting in the browser cache by the time the animation is actually invoked.  Go through your CSS files for the images used in your styling that are running slowly, and use the [Image.prefetch()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Image.html#prefetch(java.lang.String)) method.

```
    Image.prefetch("images/corner-bl.png");
    Image.prefetch("images/corner-br.png");
    Image.prefetch("images/corner-tl.png");
    Image.prefetch("images/corner-tr.png");
```
