The goals for Installomator are:

- work with various common archive types
- verify the downloaded archive or application
- have a simple ‘interface’ to the admin
- single script file so it can ‘easily’ be copied into a management system
- signed and notarized pkg-installer for local installation
- extensible without deep scripting knowledge
- work independently of a specific management system
- no dependencies that may be removed from macOS in the future or are not pre-installed

In Detail:

## Archive types

Installomator can work with the following common archive and installer types:

- pkg: as a direct download, or when nested inside a dmg or zip
- dmg: for the common 'drag app to /Applications' installation style
- zip: the application is just compressed with zip or or tbz

When the download yields a pkg file, Installomator will run `installer` to install it on the current system.

Applications in dmgs or zips will be copied to `/Applications` and their owner will be set to the current user, so the install works like a standard drag'n drop installation. (The `SYSTEMOWNER` variable controls this behavior and the owner can also be set to root/wheel.)

(I consider it a disgrace, that Jamf Pro, after more than 20 years, _still_ cannot deal with drag-and-drop installation dmgs natively. It’s not _that_ hard.)

## Verify the download

Installomator uses the macOS built-in verification for the downloads. Even though macOS quarantine does not usually affect scripted downloads and installations, we _can_ use Gatekeeper's verification with the `spctl` command.

All Installomator downloads and installations have to be signed _and_ notarized by a valid Apple Developer ID. Signature and notarization will verify the origin and that the archive has not been tampered with in transfer. In addition, the script compares the developer team ID (the ten-digit code associated with the Apple Developer account) with an _expected_ team ID. Software signed with a different, presumably malicious, Apple ID will _not_ be installed.

## Simple interface

When used to install software, Installomator has a single argument: the label or name of the software to be installed.

```
./Installomator.sh firefox
./Installomator.sh firefox LOGO=jamf BLOCKING_PROCESS_ACTION=tell_user_then_kill NOTIFY=all
```

There is a debug mode and other settings that can be controlled with variables in the code or provide by arguments. This simplifies the actual use of the script from within a management system.

## Extensible

Installomator knows how to download and install hundreds of applications and tools. You can add more by adding new labels to the `fragments`-folder.

Below is an example of a label, and most of them (just) need this information (not really "just" in this case, as we have to differentiate between arm64 and i386 versions for both `downloadURL` and `appNewVersion`):

```
firefoxpkg)
    name="Firefox"
    type="pkg"
    downloadURL="https://download.mozilla.org/?product=firefox-pkg-latest-ssl&os=osx&lang=en-US"
    firefoxVersions=$(curl -fs "https://product-details.mozilla.org/1.0/firefox_versions.json")
    appNewVersion=$(getJSONValue "$firefoxVersions" "LATEST_FIREFOX_VERSION")
    expectedTeamID="43AQ936H96"
    blockingProcesses=( firefox )
    ;;
```

When you know how to extract these pieces of information from the application, its webpage, and/or download, then you can add an application to Installomator.

The script `buildLabel.sh` can help with the label creation. Just server the URL to the script, and it will try to figure out things and write out a label as output. See [Wiki Tutorials](https://github.com/Installomator/Installomator/wiki#tutorials).

Please note: Labels should be named in small caps, numbers 0-9, “-”, and “_”. No other characters allowed. (Actually labels are part of a case-statement, and must be formatted accordingly.)

## Not specific to a management system

Installomator was originally written for use as Jamf Pro script policy. For testing, you can also run the script interactively from the command line. However, Jamf specific behavior has been kept flexible enough that the script will work independently from Jamf and with other management systems. Even if it does not work with your management system ‘out of the box’, the adaptations should be straightforward.

Not all MDMs can include the full script, for those MDMs it might be more useful to install and run the script locally on the client machines. The project provides [a signed and notarized installation pkg](Installomator/Installomator/Releases/latest) for this purpose.

## No dependencies

The script started out as a pure `sh` script, and when arrays was needed it was ‘switched’ to `zsh`, because that is what [we can rely on being in macOS for the foreseeable future](https://scriptingosx.com/zsh). There are quite a few places where using python would have been easier and safer, but with the python 2 run-time being deprecated and now removed from macOS, that would have added a dependency for a Python 3 run-time to be installed.

Keeping the script as a `zsh` allows you to simply paste the script into your management system's interface (and disable the DEBUG mode) and use it without requiring any other installations. Reducing dependencies also increases security, as the script is the only resource you need to review.