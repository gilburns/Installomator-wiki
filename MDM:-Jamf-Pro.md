In Jamf Pro, create a new ‘Script’ and paste the contents of `Installomator.sh` into the ‘Script Contents’ area.

Remember to set `DEBUG` to `0`.

Then a policy is created, that will use this script, and fill out the various parameters in the fields. The first field has to be the label (which is the name of the software), and the other fields can be the variables you need to customise the functionality. Something like this:

![Jamf Pro Policy](img/Jamf%20Pro%20Policy.png)

You would create one policy per software title, and maybe you would use different policies for self service (as these often should have `NOTIFY=all`.

Keep in mind if you setup helper text like naming parameter 4 "DEBUG" and parameter 5 "NOTIFY" then you must fill out parameters sequentially. For example, you can leave parameter 6 blank but you must fill out parameter 4 so that parameter 5 is passed to Installomator