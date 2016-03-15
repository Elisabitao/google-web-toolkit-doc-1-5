# How do I know which GWT modules I need to inherit? #
GWT libraries are organized into modules. The standard modules contain big pieces of functionality designed to work independently of each other. By selecting only the modules you need for your project (for example the JSON module rather than the XML module), you minimize complexity and reduce compilation time.

Generally, you want to inherit at least the User module. The User module contains all the core GWT functionality, including the EntryPoint class. The User module also contains reusable UI components (widgets and panels) and support for the History feature, Internationalization, DOM programming, and more.

## Standard Modules GWT 1.5 ##
| **Module** | **Logical Name** | **Module Definition** | **Contents** |
|:-----------|:-----------------|:----------------------|:-------------|
| User       | com.google.gwt.user.User | User.gwt.xml          | Core GWT functionality|
| HTTP       | com.google.gwt.http.HTTP | HTTP.gwt.xml          | Low-level HTTP communications library |
| JSON       | com.google.gwt.json.JSON | JSON.gwt.xml          | JSON creation and parsing |
| JUnit      | com.google.gwt.junit.JUnit | JUnit.gwt.xml         | JUnit testing framework integration |
| XML        | com.google.gwt.xml.XML | XML.gwt.xml           | XML document creation and parsing |

GWT 1.5 also provides several _theme_ modules which contain default styles for widgets and panels. You can specify one theme in your project's module XML file to use as a starting point for styling your application, but you are not required to use any of them.

## Themes ##
| **Module** | **Logical Name** | **Module Definition** | **Contents** |
|:-----------|:-----------------|:----------------------|:-------------|
| Chrome     | com.google.gwt.user.theme.chrome.Chrome | Chrome.gwt.xml        | Style sheet and images for the Chrome theme. |
| Dark       | com.google.gwt.user.theme.dark.Dark | Dark.gwt.xml          | Style sheet and images for the Dark theme. |
| Standard   | com.google.gwt.user.theme.standard.Standard | Standard.gwt.xml      | Style sheet and images for the Standard theme. |

## How To ##
To inherit a module, edit your project's module XML file and specify the logical name of the module you want to inherit in the `<`inherits`>` tag.

```
  <inherits name="com.google.gwt.junit.JUnit"/>
```

**Note:**
Modules are always referred to by their logical names.
The logical name of a module is of the form _pkg1.pkg2.ModuleName_ (although any number of packages may be present).
The logical name includes neither the actual file system path nor the file extension.
