# Undefined DISPLAY errors #
You might receive undefined DISPLAY errors when compiling or testing in a console or headless environment.

In a UNIX environment, GWT tests (and in some cases the GWT compiler) require an X display. If the continuous build is being run from a cron job, the running process may not have access to a "real" display.

### Workaround ###
You can provide a virtual display by using a program named Xvfb. (Xvfb stands for X11 Virtual FrameBuffer.)

To provide a virtual display:
  1. Install Xvfb.
  1. Start the Xvfb server and set the environment variable DISPLAY to use it.

```
$PP_OFF
export DISPLAY=:2

# Run Xvfb without access control on display :2  

# This command will only start Xvfb if it is not already started.
# Any GWT compiles and tests should set DISPLAY=:2 to use the virtual frame buffer.
ps -ef | grep Xvfb | grep -v grep >> /dev/null || Xvfb :2 -ac &
```
