## Mosyle (Manager, Business, and Fuse)

In this solution we recommend to use Installomator as a supplement to Mosyle Catalog. Mosyle Catalog is somewhat easier and can automatically update the outdated apps. But Mosyle does not offer as much software in their catalog as we have, so Installomator has it’s own right. And maybe you just want the extended control from using Installomator.

There are several ways to implement Installomator. One is to install it as a pkg (that we already signed and notarized) on the fleet of Macs, using “Install PKG”. For deployment it can also be part of the DEP profile. Then call this installation from “Custom Commands”.

But the pkg can also be installed as a “Custom Command” using variables, where the Installomator.pkg from the CDN is added to the script for installation. That script is “MDMMosyle Install.sh“, where the CDN-variable has to be filled out as well as the software labels that should be installed.
![Mosyle Custom Commands](img/Mosyle%20Custom%20commands.png)

Look for the scripts to be used inside the MDM solution in the MDM-folder.

Use the “App script.sh” for the subsequent updates or the Self Service display (these can be very nice by extracting the icon from the software, and add that to the “Custom Command” in Mosyle.

This script can be set to automatically run on the clients, like this:
![Mosyle Execution settings](img/Mosyle%20Execution%20settings.png)

Also see [Methods to Run Installomator](Methods to Run Installomator) for the description of the rest of the files in the MDM-folder.
