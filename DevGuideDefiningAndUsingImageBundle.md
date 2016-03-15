# Creating and Using an Image Bundle #

To define an image bundle, the user needs to extend the [ImageBundle](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/ImageBundle.html) interface. The ImageBundle interface is a tag interface that can be extended to define new image bundles. The derived interface can have zero or more methods, where each method must have the following characteristics:

  * The method takes no parameters
  * The method has a a return type of [AbstractImagePrototype](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/AbstractImagePrototype.html)
  * The method may have an optional `gwt.resource` metadata tag which specifies the name of the image file in the module's classpath


Valid image file types are `png`, `gif`, and `jpg`. If the image name contains '`/`' characters, it is assumed to be the name of a resource on the classpath, formatted as would be expected by `ClassLoader.getResource(String)`. Otherwise, the image must be located in the same package as the user-defined image bundle.

If the `gwt.resource` metadata tag is not specified, then the following assumptions are made:

  * The image filename is assumed to match the method name
  * The extension is assumed to be either `.png`, `.gif`, or `.jpg`
  * The file is assumed to be in the same package as the derived interface

In general, you should not create create the same image filename with different types.  In the event that there are multiple image files with the same names but different extensions, the order of extension precedence is (1) `png`, (2) `gif`, then (3) `jpg`.  This means that if you add `foo` to the ImageBundle and have files `foo.png` and `foo.jpg`, only `foo.png` will be picked up - `foo.jpg` will be ignored.

An image bundle for icons in a word processor application could be defined as follows:


```
public interface WordProcessorImageBundle extends ImageBundle {

  /**
   * Would match the file 'new_file_icon.png', 'new_file_icon.gif', or
   * 'new_file_icon.png' located in the same package as this type.
   */
  public AbstractImagePrototype new_file_icon();

  /**
   * Would match the file 'open_file_icon.gif' located in the same
   * package as this type.
   * 
   * @gwt.resource open_file_icon.gif
   */
  public AbstractImagePrototype openFileIcon();

  /**
   * Would match the file 'savefile.gif' located in the package
   * 'com.mycompany.mygwtapp.icons', provided that this package is part
   * of the module's classpath.
   * 
   * @gwt.resource com/mycompany/mygwtapp/icons/savefile.gif
   */
  public AbstractImagePrototype saveFileIcon();
}
```

Methods in an image bundle return [AbstractImagePrototype](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/AbstractImagePrototype.html) objects rather than [Image](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/Image.html) objects, as you might have expected.  This is because `AbstractImagePrototype` objects provide additional lightweight representations of an image.  For example, the [AbstractImagePrototype.getHTML()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/AbstractImagePrototype.html#getHTML()) method provides an HTML fragment representing an image without having to create an actual instance of the `Image` widget. In some cases, it can be more efficient to manage images using these HTML fragments.

Another use of `AbstractImagePrototype` is to use [AbstractImagePrototype.applyTo(Image)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/AbstractImagePrototype.html#applyTo(com.google.gwt.user.client.ui.Image)) to transform an existing `Image` into one that matches the prototype without having to instantiate another `Image` object. This can be useful if your application has an image that needs to be swapped depending on some user-initiated action.

Not all situations can be satisfied with a lightweight HTML fragment or by copying into an existing `Image`.  For example, you may need to access a member of your Image bundle and pass an `Image` object into a `setWidget()` method to display your image in a `TreeItem`.  In those cases, the [AbstractImagePrototype.createImage()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/AbstractImagePrototype.html#createImage()) method can be used to generate new `Image` instances.

The following example shows how to use the image bundle that we just defined in your application.  An `Image` object is needed in this case because the `HorizontalPanel` performs layout only on subclasses of `Widget`.

```
public void useImageBundle() {
  WordProcessorImageBundle wpImageBundle = (WordProcessorImageBundle) GWT.create(WordProcessorImageBundle.class);
  HorizontalPanel tbPanel = new HorizontalPanel();
  tbPanel.add(wpImageBundle.new_file_icon().createImage());
  tbPanel.add(wpImageBundle.openFileIcon().createImage());
  tbPanel.add(wpImageBundle.saveFileIcon().createImage());
}
```

> _Tip: Image bundles are immutable, so you can keep a reference to a singleton instance of an image bundle instead of creating a new instance every time the image bundle is needed._


### See Also ###

  * [ImageBundle](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/ImageBundle.html)

  * [AbstractImagePrototype](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/AbstractImagePrototype.html)


