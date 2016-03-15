# Command-line Tools #
The Google Web Toolkit includes a set of scripts that can be run from the command-line.

## Tools for creating and organizing project components ##
These tools generate the project files you need to get a project off the ground quickly or to add new components to existing projects.

[applicationCreator](DevGuideApplicationCreator.md)
> Generate a starter application, including launch scripts.

[benchmarkViewer](DevGuideBenchmarkViewer.md)
> Display benchmark results.

[i18nCreator](DevGuideI18nCreator.md)
> Generate an i18n properties file and synchronization script for an existing project.

[junitCreator](DevGuideJunitCreator.md)
> Generate a JUnit test for an existing project.

[projectCreator](DevGuideProjectCreator.md)
> Generate an Ant buildfile and/or Eclipse project.

## Run scripts ##
If you create your project files using applicationCreator, it creates two scripts you can use to run your application. Each script is prefixed by the application name: `<`module`>`-compile and `<`module`>`-shell; for example, for a project named StockWatcher the scripts generated are StockWatcher-compile and StockWatcher-shell.

[compile script](DevGuideModuleCompileScript.md)
> Compile the Java code in the named module into JavaScript and run it in [web mode](DevGuideRunningInWebMode.md).

[hosted mode shell script](DevGuideModuleHostedModeScript.md)
> Run the named module in [hosted mode](DevGuideHostedMode.md).
<a href='Hidden comment: 
2008-11-30. This info is also in GettingStartedInstall in 1.5 which won"t exist in 1.6.
Move here so it isn"t lost.
'></a>
## Adding the GWT application directory to your PATH environment variable ##
In order to invoke the command-line tools without entering their full path names, add them to your system's command search path. To do this, in the PATH environment variable, identify the directory in which you unpacked the GWT distribution.

### Windows ###
  1. Right-click on _My Computer_ and select _Properties_
  1. Open the _Advanced_ tab.
  1. Click the _Environment Variables_ button.
  1. From the user variables list, select _Path_ and click _Edit_
  1. At the end of the of the variable value, add a semicolon followed by the full path to the directory where you unpacked the GWT distribution (e.g., `;C:\gwt-windows-1.5.3\`).

### Mac or Linux ###
Edit a file named `.profile` or `.bash_profile` in your home directory. For example, if you unpacked GWT in `/home/user/gwt-linux-1.5.3/`, update your profile as follows:
```
$PP_OFF
PATH=$PATH:/home/user/gwt-linux-1.5.3/
export PATH
```
You will need to log out of your account and log back in before the PATH setting takes effect.
