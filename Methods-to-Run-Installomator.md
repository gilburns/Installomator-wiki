## MDM solution with full script

Especially Jamf Pro can easily handle a very large script, like Installomator, and run that on managed Macs and at the same time calling the script with various parameters. 

Other MDM solutions might be able to do the same, but Installomator can also be locally installed and just vcalled from there with a much smaller script.

## MDM, pkg-installed (running locally)

Many MDM solutions can run scripts, but not as large a script as Installomator has become. For these solutions we recommend to install the script on the managed Macs, and call it with a smaller script from the MDM.

For these kinds of solutions we have provided both the pkg-file (signed and notarized), so it can even be installed as part of the DEP profile.

Look for the scripts to be used inside the MDM solution in the MDM-folder.

The scripts “Installomator update.sh” is for updating the installation of Installomator, but grabbing the latest version from GitHub, and install that PKG.

The script “Manual valuesfromarguments.sh” is an example of how to install a custom software title, where you put in the values for the software manually.

A script “RemoveInstallomator.sh” has been provided to remove Installomator completely from a Mac.
