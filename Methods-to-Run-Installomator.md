## MDM, Jamf Pro

In Jamf Pro you would add the full script to the script list in the settings area.

Then a policy is created, that will use this script, and fill out the various parameters in the fields. The first field has to be the label (which is the name of the software), and the other fields can be the variables you need to customise the functionality. Something like this:

![Jamf Pro Policy](https://user-images.githubusercontent.com/1933192/141141579-779e38b3-f15a-4335-b873-70b27389e6e7.png)

You would create one policy per software title, and maybe you would use different policies for self service (as these often should have `NOTIFY=all`.

## MDM, pkg-installed (running locally)

Many MDM solutions can run scripts, but not as large a script as Installomator has become. For these solutions we recommend to install the script on the managed Macs, and call it with a smaller script from the MDM.

For these kinds of solutions we have provided both the pkg-file (signed and notarized), so it can even be installed as part of the DEP profile.

Look for the scripts to be used inside the MDM solution in the MDM-folder.

The scripts “Installomator update.sh” is for updating the installation of Installomator, but grabbing the latest version from GitHub, and install that PKG.

The script “Manual valuesfromarguments.sh” is an example of how to install a custom software title, where you put in the values for the software manually.

A script “RemoveInstallomator.sh” has been provided to remove Installomator completely from a Mac.

### Mosyle (Manager, Business, and Fuse)

In this solution we recommend to use Installomator as a supplement to Mosyle Catalog. Mosyle Catalog is somewhat easier and can automatically update the outdated apps. But Mosyle does not offer as much software in their catalog as we have, so Installomator has it’s own right. And maybe you just want the extended control from using Installomator.

There are several ways to implement Installomator. One is to install it as a pkg on the fleet of Macs, using “Install PKG”. For deployment it can also be part of the DEP profile.

But it can also be installed as a “Custom Command” using variables, where the Installomator.pkg from the CDN is added to the script for installation. That script is “MDMMosyle Install.sh“, where the CDN-variable has to be filled out as well as the software labels that should be installed.

Use the “App script.sh” for the subsequent updates or the Self Service display (these can be very nice by extracting the icon from the software, and add that to the “Custom Command” in Mosyle.

### Addigy

Addigy has a large built in software catalog, but the updates has to be manually sent out to clients, and if this needs to be automated, Installomator is a great addition. Addigy has a condition script before installing software, and it will use this as a check before allowing the script/the installation to run. In Self Service if that condition script is not right, the software cannot be clicked to be installed (this is pretty neat).

Create a custom software installer to install the Installomator PKG, as well as run the looping software installation of all the software needed. This should not be shown in Self Service. This is the “MDMAddigy CustomSoftware.sh” script.

Individual custom software titles can be built using the “App script.sh”, with icons and all.

If automatic updates should be run, make som Maintenance scripts for this. Better yet, make some Scripts for each software title, so it can also be used as a manual script to install the software on a given Mac. The maintenance script could have a line for investigating if the given software is installed, and exit if it is not (so it will only be updated if it is actually installed).

### Endpoint Manager, Microsoft Intune

Scripts can be run from here. Currently not testet, but likely use would be to install the PKG on the clients, and use the “App script.sh” and “App-loop script.sh” for the software installations.

### GrappleMDM

This solution choose to implement Installomator directly into the solution. You don't have to do anything yourself.
