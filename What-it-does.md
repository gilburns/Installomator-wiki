## What it does

When Installomator runs with a known label, the script will perform the following:

- Check the version installed with the version online. Only continue if it's different
- download the latest version from the vendor
- when the application is running, prompt the user to quit or cancel (customizable)
- dmg or other archives:
    - extract the application and copy it to /Applications
    - change the owner of the application to the current user (optional)
- pkg files:
    - when necessary, extract the pkg from the enclosing archive
    - install the pkg with the `installer` tool
- clean up the downloaded files
- notify the user (also customizable)
- restart the app (optional)
