# Image Bundles #
An _image bundle_ is a construct used to improve application performance by reducing the number of round trip HTTP requests to the server to fetch images. GWT can package many image files into a single large file to be downloaded from the server and managed as a Java object.

## Topics ##
[ImageBundles Make Using Images More Efficient](DevGuideImageBundleMotivation.md)
> Explains why Image Bundles are more efficient than using HTML IMG tags.

[Creating and Using an Image Bundle](DevGuideDefiningAndUsingImageBundle.md)
> Define an image bundle and use it in your application.

[Image Bundles and Localization](DevGuideImageBundleWithLocalization.md)
> Create locale-sensitive image bundles by using GWT's localization capabilities.

## See Also ##
See also, the GWT API Reference, the [ImageBundle](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/ImageBundle.html) interface.

