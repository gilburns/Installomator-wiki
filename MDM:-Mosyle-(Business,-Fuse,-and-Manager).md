In this solution we recommend to use Installomator as a supplement to Mosyle Catalog. Mosyle Catalog is somewhat easier and can automatically update the outdated apps. But Mosyle does not offer as much software in their catalog as we have, so Installomator has it’s own right. And maybe you just want the extended control from using Installomator.

With Mosyle, you can install Installomator locally by either uploading the PKG to your Mosyle CDN and using "Install PKG", or by using a script as a Custom Command to download and install Installomator.

![Mosyle Custom Commands](img/Mosyle%20Custom%20commands.png)

We have a README and several example scripts in the [Installomator repo's MDM folder](https://github.com/Installomator/Installomator/tree/main/MDM).

Use the “App script.sh” for the subsequent updates or the Self Service display (these can be very nice by extracting the icon from the software, and add that to the “Custom Command” in Mosyle.

This script can be set to automatically run on the clients, like this:

![Mosyle Execution settings](img/Mosyle%20Execution%20settings.png)
