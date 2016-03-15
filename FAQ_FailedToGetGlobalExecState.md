# Failed to getGlobalExecState #
On Mac when launching hosted mode, receive the error "Failed to getGlobalExecState".

This problem is related to the Google Gears enabler.

### Workaround ###
Check the /Library/InputManagers folder. If there is a file named GearsEnabler.bundle, removing it should fix this issue.
