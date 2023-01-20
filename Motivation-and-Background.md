## Motivation and Background

In the world of managing Apple Macs, organizations can have two different approaches to the management. Either the IT department will tightly manage and verify each piece of software, or they will just want the latest software to be deployed as fast as possible.

OK, maybe some software should be tightly managed and others not, but you get the point.

### Tightly managed

If your solution needs to be tightly managed, i.e. the versions of the operating system and third party software are controlled and updates will only be pushed with the management system when the administration and security team went through an approval process and then the update is automated. This is an important and valid workflow and the right fit for many deployments.

Installomator was _not_ written for these kinds of deployment.

If you are running this kind of deployment, you want to use [AutoPkg](https://github.com/autopkg/autopkg) and you can stop reading here.

### Latest version always

There are other kinds of deployments, though. In these deployments the management system is merely used to “get the user ready” as quickly as possible when they set up a new machine, and to offer software from a self service portal. In these deployments, system and software installations are ‘latest version available’ and updates are user driven (though we may want to nag them).

This is where Installomator fits in.

These deployments are

- user driven
- low control
- minimal maintenance effort
- latest version

These can be 'user controlled' Macs and we (the admins) just want to assist the user in doing the right thing, which is (often) to install the latest versions and updates when they are available.

The Mac App Store and managed software pushed through the Mac App Store (VPP/"Apps & Books") follow this approach. When you manage and deploy software through the App Store/"Apps & Books" — whether it is on iOS or macOS — neither the MacAdmin nor the user get a choice of the application version. They will get the latest version.

In such deployments, keeping the installers hosted in your management system up to date is an extra burden. AutoPkg can, well, automate much of the download/re-package/upload/stage cycle, but it still requires oversight and maintenance. Instead of downloading, re-packaging, uploading application installers to the management system, it is often easier to run a script which downloads the latest version directly from the vendor's servers and installs it.

There are obviously a few downsides to this approach:

- when your fleet is mostly on site and many Macs install or update at the same time, they will reach out over the internet to the vendor's servers, possibly overwhelming your internet connection
- when you download software from the internet, it has to be verified to avoid man-in-the-middle or other injection attacks
- there is no control over which version the clients get, you cannot "hold back" new versions for testing and approval workflows
- some application downloads are gated behind logins or paywalls and cannot be automated this way

Some of these disadvantages can be seen as advantages in different setups. When your fleet is mostly mobile and offsite, then downloading from vendor servers will relieve the inbound connection to your management server, or the data usage on your management system's cloud server. Software vendors are pushing for subscriptions with continuous updates and feature releases, and moving the entire team to the latest versions quickly can make those available quickly. Also being on the latest release includes all current security patches.

Because this is an attractive solution for _certain kinds_ of deployment, there have always been many scripts out there that will download and install the latest version of a given software. And we have built and used quite a few in-house, as well. Most importantly, [William Smith has this script](https://gist.github.com/talkingmoose/a16ca849416ce5ce89316bacd75fc91a) which can be used to install several different Microsoft applications and bundles, because Microsoft has a nice unified URL scheme.

At some point, in 2018, I (Armin, @scriptingosx) got frustrated at the number of scripts we were maintaining (or failing to). Also, most of the scripts were not verifying the download at all. So, I set out to write _the one install script to rule them all…_

### Locally installed

Originally, Installomator was built to work as a Script in Jamf Pro policies. But the idea that it should be designed in a way that it can run locally or from other management system was always part of the design

Søren (@Theilgaard) adapted Installomator to work with Mosyle and Addigy. For these solutions, Installomator had to be locally installed on the Mac. Then the MDM can call this script from their scripts features. Søren created a forked version of Installomator a notarized pkg, so it could be deployed as part of DEP or however was needed. This has now been merged into Installomator, and with contributions from Isaac and Adam, new features and labels have been added more frequently.
