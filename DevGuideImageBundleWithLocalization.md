# Image Bundles and Localization #

Sometimes applications need different images depending on the locale that the user is in. When using image bundles, this means that we need different image bundles for different locales.

Although image bundles and localization are orthogonal concepts, they can work together by having locale-specific factories create instances of image bundles.
The best way to explain this technique is with an example. Suppose that we define the following `ImageBundle` for use by a mail application:


```
public interface MailImageBundle extends ImageBundle {

  /**
   * The default 'Compose New Message' icon if no locale-specific
   * image is specified.
   *
   * @gwt.resource compose_new_message_icon.gif
   */
  public AbstractImagePrototype composeNewMessageIcon();

  /**
   * The default 'Help' icon if no locale-specific image is specified.
   * Will match 'help_icon.png', 'help_icon.gif', or 'help_icon.jpg' in
   * the same package as this type.
   */
  public AbstractImagePrototype help_icon();
}
```

Suppose the application has to handle both English and French users. In that case, we will have to define these locale values in the [module XML file](DevGuideModuleXml.md):

```
<module>

	<!-- Inherit the core Web Toolkit stuff.      -->
	<inherits name='com.google.gwt.user.User' />
	<inherits name='com.google.gwt.i18n.I18N' />

	<!-- For Localizable                          -->
        <extend-property name='locale' values='fr' />
        <extend-property name='locale' values='en' />

	<!-- Other settings ...                       -->

  
</module>
```

We define English and French variations of each image in `MailImageBundle` by creating locale-specific image bundles that extend `MailImageBundle`:

```
public interface MailImageBundle_en extends MailImageBundle {

  /**
   * The English version of the 'Compose New Message' icon.
   * Since we are not overriding the help_icon() method, this bundle
   * uses the inherited method from MailImageBundle.
   *
   * @gwt.resource compose_new_message_icon_en.gif
   */
  public AbstractImagePrototype composeNewMessageIcon();


  /* Note: No override for the help icon */

}
```

```
public interface MailImageBundle_fr extends MailImageBundle {

  /**
   * The French version of the 'Compose New Message' icon.
   *
   * @gwt.resource compose_new_message_icon_fr.gif
   */
  public AbstractImagePrototype composeNewMessageIcon();

  /**
   * The French version of the 'Help' icon.
   *
   * @gwt.resource help_icon_fr.gif
   */
  public AbstractImagePrototype help_icon();
}
```

The next step is to create a mechanism for choosing the correct image bundle based on the user's locale. By extending [Localizable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Localizable.html), we can create a locale-sensitive factory that will instantiate `MailImageBundle` with deferred binding.

```
public interface MailImageBundleFactory extends Localizable {

  public MailImageBundle createImageBundle();
}
```

The factory methods are instantiating the `ImageBundle` superclass using deferred binding.  Invoking the  [GWT.create()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class) method will cause a generator to create Java code to implement the details for the image bundle class for each factory:

```
public class MailImageBundleFactory_en implements MailImageBundleFactory {

  public MailImageBundle createImageBundle() {
    return (MailImageBundle) GWT.create(MailImageBundle_en.class);
  }
}

```

```
public class MailImageBundleFactory_fr implements MailImageBundleFactory {

  public MailImageBundle createImageBundle() {
    return (MailImageBundle) GWT.create(MailImageBundle_fr.class);
  }
}
```

Finally, we need to create a Factory to return the default implementation of our ImageBundle. Do this by naming the class with the same prefix as the interface, but ending with an underscore character:

```
public class MailImageBundleFactory_ implements MailImageBundleFactory {

  public MailImageBundle createImageBundle() {
    return (MailImageBundle) GWT.create(MailImageBundle.class);
  }
}
```

Application code that utilizes a locale-sensitive image bundle might look something like this:

```
public void useLocalizedImageBundle() {
  // Create a locale-sensitive MailImageBundleFactory
  MailImageBundleFactory mailImageBundleFactory = (MailImageBundleFactory) GWT
      .create(MailImageBundleFactory.class);

  // This will return a locale-sensitive MailImageBundle, since we are using
  // a locale-sensitive factory to create it.
  MailImageBundle mailImageBundle = mailImageBundleFactory.createImageBundle();

  // Get the image prototype for the icon that we are interested in.
  AbstractImagePrototype helpIconProto = mailImageBundle.help_icon();

  // Create an Image object from the prototype and add it to a panel.
  HorizontalPanel panel = new HorizontalPanel();
  panel.add(helpIconProto.createImage());
}
```

Creating the factory class also takes advantage of GWT's [deferred binding](DevGuideDeferredBinding.md) feature, this time based on the `Localizable` base class.  The [GWT.create(Class)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class)) method instructs the compiler to choose one of the several `locale` specific subclasses of `MailImageBundleFactory` available.

## Why did this Example use Deferred Binding Twice? ##

If you are familiar with deferred binding, you might be thinking, _Why didn't we just make `MailImageBundle` extend the `Localizable` class directly?_

The answer is rooted in the fact that you cannot extend more than one class in Java.   Both `ImageBundle` and the `Localizable` class are needed to implement this feature and they both use deferred binding.  Hence the use of the _Factory method pattern_.

### See Also ###

  * [ImageBundle](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/ui/ImageBundle.html)

  * [Localizable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Localizable.html)


