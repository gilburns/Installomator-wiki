In Jamf Pro, create a new ‘Script’ and paste the contents of `Installomator.sh` into the `Script Contents` area.

Remember to set `DEBUG` to `0`.

Then a policy is created, that will use this script, and fill out the various parameters in the fields. The first field has to be the label (which is the name of the software), and the other fields can be the variables you need to customise the functionality. Something like this:

![Jamf Pro Policy](img/Jamf%20Pro%20Policy.png)

You would create one policy per software title, and maybe you would use different policies for self service (as these often should have `NOTIFY=all`.

Keep in mind if you setup helper text like naming parameter 5 "DEBUG" and parameter 6 "NOTIFY" then you must fill out parameters sequentially. For example, you can leave parameter 7 blank when not used, but you must fill out parameter 5 so that all filled out parameters are passed to Installomator.

We have a README and several example scripts in the [Installomator repo's MDM folder](https://github.com/Installomator/Installomator/tree/main/MDM) which can be used with Jamf. Additionally, we have Jamf example scripts in the Jamf subfolder. 