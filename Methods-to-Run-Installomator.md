## MDM policy with Installomator script

MDMs like Jamf Pro will have a configured policy with a script configured in it, like the Installomator script, and the script will run when the policy runs.

## MDM script with locally installed Installomator

Many MDM solutions can run scripts, but not as large a script as Installomator has become. For these solutions we recommend to install the script on the managed Macs, and call it with a smaller script from the MDM.

For these kinds of solutions we have provided a signed and notarized PKG, [see Releases.](https://github.com/Installomator/Installomator/releases/).

We have a README and several example scripts in the [Installomator repo's MDM folder](https://github.com/Installomator/Installomator/tree/main/MDM).