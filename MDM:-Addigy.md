Addigy has a large built in software catalog, but the updates has to be manually sent out to clients, and if this needs to be automated, Installomator is a great addition. Addigy has a condition script before installing software, and it will use this as a check before allowing the script/the installation to run. In Self Service if that condition script is not right, the software cannot be clicked to be installed (this is pretty neat).

Create a custom software installer to install the Installomator PKG, as well as run the looping software installation of all the software needed. This should not be shown in Self Service. This is the “MDMAddigy CustomSoftware.sh” script.
![Addigy Custom software](img/Addigy%20Custom%20software.png)

Look for the scripts to be used inside the MDM solution in the MDM-folder.

Individual custom software titles can be built using the “App script.sh”, with icons and all.

If automatic updates should be run, make som Maintenance scripts for this. Better yet, make some Scripts for each software title, so it can also be used as a manual script to install the software on a given Mac. 
![Addigy Script](img/Addigy%20Script.png)

The maintenance script could have a line for investigating if the given software is installed, and exit if it is not (so it will only be updated if it is actually installed).

Also see [Methods to Run Installomator](Methods-to-Run-Installomator) for the description of the rest of the files in the MDM-folder.
