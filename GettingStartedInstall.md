# Install GWT #

## Install the Java SDK ##
If you don't have a recent version of the Java SDK installed (version 1.5 or higher), download and install the [Sun Java Standard Edition SDK](http://java.sun.com/javase/downloads/).

## Download GWT ##
Download the [GWT distribution](http://code.google.com/webtoolkit/download.html) for your operating system.

## Unzip the GWT distribution ##
On Windows, extract the files from `gwt-windows-1.5.3.zip` with a program like [WinZip](http://www.winzip.com/).

On Mac, you can unpack the distribution with a command like:
```
$PP_OFF
tar xvzf gwt-mac-1.5.3.tar.gz
```

On Linux, use the command:
```
$PP_OFF
tar xvf gwt-linux-1.5.3.tar.bz2
```

That's all there is to it!  GWT doesn't use an installer application; all the files you need to run and use GWT are located in the extracted directory.

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
