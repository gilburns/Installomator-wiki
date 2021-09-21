Example: Microsoft Azure Storage Explorer

So a user was trying to make a label for a github.com app from Microsoft. The repository is here:
[AzureStorageExplorer](https://github.com/microsoft/AzureStorageExplorer)

Installomator has buil in support for github repositories, we almost just need to call them with their user-name and repository-name.

But we need to know the name of the archive, as well as knowing what archive it is. 

__Please note:__ `buildLabel.sh` is no help for this kind of label.

## github.com structure

We know some certain links on github to always be there:
[All releases: releases](https://github.com/microsoft/AzureStorageExplorer/releases)
[Latest release: releases/latest](https://github.com/microsoft/AzureStorageExplorer/releases/latest)

In order to figure out the rest of the label contruction, we need to see how the latest release looks. So go to that URL now.

We can see that the release has 3 binaries; Linux, Mac, and Windows. We can also see that the Mac archive is a zip archive. 
`type="zip"`

## github fun

__For fun__, try to go to the releases of the [Wally-label in Tutorial 3](https://github.com/zsa/wally/releases). The latest release is right now only a Linux version, and the latest Mac release is somewhere down the list. We cannot use the built in tools in Installomator for github in this situation, as they have not aligned the releases so that the Mac version will be released in each release, and Installomator would fail (the app is also versioned incorrectly, but that is another story to be read in that tutorial).

## archive name on github

In the current case, it could be argued that zip would be enough to find the Mac version among the Linux and Windows versions, as that is the only zip archive, but we can make certain to hit the Mac release by using the variable `archiveName` by specifying the name of the archive.

But take a moment and go through the most recent releases and verify for yourself that the Mac archive have been named like this in these releases. If that was not the case, we might simply just let installomator detect the zip archive, and hope no other platform version would be relased in a type like that.

But we can safely use this (I expect, but we have no idea if something changes in the future):
`archiveName="Mac_StorageExplorer.zip"`

Download that latest version now, and expand that so we can detect it's name (without the .app extension):
`name="Microsoft Azure Storage Explorer"` 

In this case we do not need the `appName` variable, as `name` is enough and will be matched by Installomator with ".app" appended to the `name` variable.

It would have been this:
`appName="Microsoft Azure Storage Explorer.app"`

## Manually detecting the TeamID

We need the `TeamID` of the app that we downloaded and expanded to ~/Downloads:
```
% codesign -display -r - Microsoft\ Azure\ Storage\ Explorer.app 
Executable=/Users/st/Downloads/Microsoft Azure Storage Explorer.app/Contents/MacOS/Microsoft Azure Storage Explorer
designated => identifier "com.microsoft.StorageExplorer" and anchor apple generic and certificate 1[field.1.2.840.113635.100.6.2.6] /* exists */ and certificate leaf[field.1.2.840.113635.100.6.1.13] /* exists */ and certificate leaf[subject.OU] = UBF8T346G9
```

It's the last "number" after the equal character in the third line:
`expectedTeamID="UBF8T346G9"`

## Blocking processes

This app looks like it’s the only process started up, so we don't need to handle any specific other process (in case we need this process to be quit or killed before we can update the app). 

So it would have been this (but that matches the original `name` so we do not need it):
`blockingProcesses=( "Microsoft Azure Storage Explorer" )`

## Final label

From the above work, we can now make this label:
```
microsoftazurestorageexplorer)
    name="Microsoft Azure Storage Explorer"
    type="zip"
    downloadURL=$(downloadURLFromGit microsoft AzureStorageExplorer )
    appNewVersion=$(versionFromGit microsoft AzureStorageExplorer )
    expectedTeamID="UBF8T346G9"
    archiveName="Mac_StorageExplorer.zip"
    ;;
```
