# International characters don't display correctly #
If you've embedded international characters into Java source files, these characters might not display correctly when you run the application and view it in a web browser.

Java requires UTF-8 for source code files containing international characters. Frequently the editor used to generate the files (such as an IDE) is not configured to save the files as UTF-8. Thus your source files are not properly encoded in UTF-8.

### Troubleshooting checklist ###
  * Check your editor to see if it is capable of saving files as UTF-8 and that it is currently configured to do so.
  * If you have embedded international characters directly into your Java source files, check to see if they are saved in UTF-8 format.
  * Check any associated Java properties files (or other files where you've stored translations) to see if they are saved in UTF-8 format.
  * Check the HTML host page. If your web page content contains localized data, encode it as UTF-8 by adding the following tag to the `<`head`>` element.

```
<meta http-equiv="content-type" content="text/html;charset=utf-8" />`
```
