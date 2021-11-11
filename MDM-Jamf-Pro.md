## MDM, Jamf Pro

In Jamf Pro you would add the full script to the script list in the settings area.

Then a policy is created, that will use this script, and fill out the various parameters in the fields. The first field has to be the label (which is the name of the software), and the other fields can be the variables you need to customise the functionality. Something like this:

![Jamf Pro Policy](https://user-images.githubusercontent.com/1933192/141141579-779e38b3-f15a-4335-b873-70b27389e6e7.png)

You would create one policy per software title, and maybe you would use different policies for self service (as these often should have `NOTIFY=all`.