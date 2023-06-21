# MDM: Jamf School

Utilizing Installomator within the limitations of Jamf School is somewhat unique, but is a fantastic way to extend the MDM. The following information provides examples of how I quickly implemented Installomator in a Jamf School MDM environment using only basic Mac Admin skills.

Requirements:

* Jamf School Scripting Module ([how-to](https://learn.jamf.com/bundle/jamf-school-documentation/page/Scripts.html))
* Installomator PKG ([download](https://github.com/Installomator/Installomator/releases/)
* Jamf Composer (or other package creation tool)

### Deploying Installomator
Simply download the [latest Installomator PKG from the GitHub repository](https://github.com/Installomator/Installomator/releases/) and upload it to Jamf School by using "Add In-House macOS Package", set the Scope, and ensure "Automatic Installation" is specified. Once deployed, the Installomator installations can be maintained by an Installomator script.

### Installomator Scripts
Before you can leverage the Installomator installations on your Macs, you will first need the Scripting module enabled in Jamf School. 

In my use case, there were remnants of previously installed applications on the Macs, which required a use of the "force" install by Installomator to achieve a new installation. Due to this, I split my approach into two categories: Installation Scripts & Maintenance Scripts. This is not required.

**Installation Scripts**

With the Scripting module enabled, click "Scripts" in the Menu, then "+Create new script". Then set the Name, Type, Description, and Content (see below). Installation Scripts should be set to "Just once" for "When to run".

For the installation scripts, I set the Content as follows:

    #!/bin/sh  
    /usr/local/Installomator/Installomator.sh microsoftoffice365 INSTALL="force"
    
In this example, I added a Smart Group of all enrolled Macs to the scope of the script. This triggers an execution of the Installomator script on all currently enrolled Macs as well as any that dynamically get added to the Smart Group.

![Installation Script Example](https://storage.googleapis.com/www-trabant-tech-com/InstallomatorJamfSchool-InstallationScript.png)

**Maintenance Scripts**

With the Scripting module enabled, click "Scripts" in the Menu, then "+Create new script". Then set the Name, Type, Description, and Content (see below). Maintenance Scripts can be scheduled to run in accordance with the frequency you would like to check for updates.

For the maintenance scripts, I set the Content as follows:

    #!/bin/sh  
    /usr/local/Installomator/Installomator.sh installomator.sh

In this example, I added a Smart Group of all Macs with Installomator installed to the scope of the script.

![Maintenance Script Example](https://storage.googleapis.com/www-trabant-tech-com/InstallomatorJamfSchool-MaintenanceScript.png)


### Jamf School Self Service
In Jamf School, I ran into "Self Service" limitations that I was able to leverage Installomator to overcome.

**Vendor Provided Installer (Zoom.app)**

Zoom offers a universal binary PKG and an Apple Silicon specific PKG. If you upload both to Jamf School, the second PKG will be rejected by Jamf as a duplicate. If you choose to use the universal PKG as provided by Zoom, Apple Silicon users will get prompts to install the other version. To circumvent this in Jamf School, I made Zoom available as an On-Demand installation. Then, I created a Smart Group for Macs with Zoom installed. Then I target the Smart Group with a Script which performs a "force" install of Zoom with Installomator.  Installomator will then update Zoom to the latest version and replace the installed universal app with the Apple Silicon app.

**Custom Installer (Spotify.app)**

Spotify does not offer a PKG or DMG that is deployable by Jamf School. To cater to end-users of both processor architectures who would like to install Spotify, I used Jamf Composer to create a custom installer PKG. The PKG is based on a "Normal Snapshot" before and after a Spotify installation. I then added a postflight script to run a "force" install with Installomator, which will update the app to the latest version and the correct architecture.