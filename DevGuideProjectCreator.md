# projectCreator #

A command line tool that generates an [Ant](http://ant.apache.org/) buildfile or [Eclipse](http://www.eclipse.org) project.

`projectCreator [-ant projectName] [-eclipse projectName] [-out dir] [-overwrite] [-ignore] [-addToClassPath]`

|   `-ant`  |  Generate an Ant buildfile to compile source (`.ant.xml` will be appended) |
|:----------|:---------------------------------------------------------------------------|
|   `-eclipse`  |  Generate an eclipse project                                               |
|   `-out`  |  The directory to write output files into (defaults to current)            |
|   `-overwrite`  |  Overwrite any existing files                                              |
|   `-ignore`  |  Ignore any existing files; do not overwrite                               |
|   `-addToClassPath` |  Adds extra elements to the class path of files in the skeleton.           |


## Example ##

```
$PP_OFF
 ~/Foo> projectCreator -ant Foo -eclipse Foo
 Created directory src
 Created directory test
 Created file Foo.ant.xml
 Created file .project
 Created file .classpath
```


Running `ant -f Foo.ant.xml` will compile `src` into `bin`. The buildfile also contains a `package` target for bundling the project into a jar.

When run with the `-eclipse` option, an additional `.project` and `.classpath` file are created.  The `.project` file can be imported into an Eclipse workspace using the `File -> Import...` dialog.

## Troubleshooting ##

  * If you are running on Windows, make sure you open a command shell first to type in the `projectCreator` command and its arguments. Running the `projectCreator` and other command line utilities will not work by clicking on them in Windows Explorer.
  * Make sure the `java` executable is on your `PATH` setting.
  * You may need to add the directory where you unpacked the GWT distribution to your `PATH` setting, or invoke the utility with a full path.

```
$PP_OFF
C:\>C:\GWT\projectCreator -ant Foo -eclipse Foo -out Foo
```

### See Also ###

  * [Getting Started](GettingStarted.md)

  * [applicationCreator](DevGuideApplicationCreator.md)

  * [Project Structure](DevGuideProjectStructure.md)