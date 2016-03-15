# InternalError: Can't connect to X11 window #
Does the GWT compiler need an X11 Window in Linux? No, the GWT compiler can run "headless" (that is, accessing the AWT library without needing to load a Graphics Environment Window).

You might run into the problem in the first place because the [ImageBundle](DevGuideImageBundles.md) feature triggers the GWT compiler to connect to an X11 Graphics Environment Window at compile time. If you don't have a DISPLAY environment variable set, it will issue the following error message:

```
$PP_OFF
java.lang.InternalError: Can't connect to X11 window server using ':0.0' as the value of the DISPLAY variable.
```

Even if you are not using the ImageBundle explicitly in your client code, you may still encounter this error message because GWT uses the ImageBundle internally for widgets (like the Tree widget) to further optimize the number of HTTP roundtrips for images and speed up your web application. This means if you're using any widgets that use ImageBundle in their underlying implementations, the GWT compiler will search for the DISPLAY environment variable and try to connect to an X11 Graphics Window.
<a href='Hidden comment: 
Do we have a list of all the widgets that use !ImageBundle in their underlying implementations somewhere?
'></a>

### Workaround ###
To avoid this error message, run the GWT compiler with the headless AWT option.
```
$PP_OFF
-Djava.awt.headless=true
```

#### Examples ####
**Ant.** To the GWT compilation build target, add the element: `<jvmarg value="-Djava.awt.headless=true"/>`

**Command-line.** Pass in the argument: `-Djava.awt.headless=true`

**Eclipse.** In your Run Configuration window, select the Arguments tab and in the VM arguments section, enter: `-Djava.awt.headless=true`

If you use a custom build process, you can also set the AWT headless option there.
