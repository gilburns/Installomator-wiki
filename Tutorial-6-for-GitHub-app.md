Example: Microsoft Azure Storage Explorer

This tutorial is ceompletely new as of 16 November 2021, as the `buildLabel.sh`-script has been improved a lot.

A user was trying to make a label for a github.com app from Microsoft. The repository is here:
[AzureStorageExplorer](https://github.com/microsoft/AzureStorageExplorer)

## github.com structure

We know some certain links on github to always be there:
[All releases: releases](https://github.com/microsoft/AzureStorageExplorer/releases)
[Latest release: releases/latest](https://github.com/microsoft/AzureStorageExplorer/releases/latest)

In order to figure out the rest of the label contruction, we need to see how the latest release looks. So go to that URL now.

We can see that the release has 3 binaries; Linux, Mac, and Windows. We can also see that the Mac archive is a zip archive. 
`type="zip"`

## `buildLabel.sh` magic

Installomator.sh has built-in support for github repositories, and now `buildLabel.sh` can build those labels very easily.

The latest version for Mac is this: [https://github.com/microsoft/AzureStorageExplorer/releases/download/v1.21.3/Mac_StorageExplorer.zip](https://github.com/microsoft/AzureStorageExplorer/releases/download/v1.21.3/Mac_StorageExplorer.zip)

So we give that URL to `buildLabel.sh` and look at this “magic”. See the GitHub section and the final label produced:
```
% Installomator/utils/buildLabel.sh "https://github.com/microsoft/AzureStorageExplorer/releases/download/v1.21.3/Mac_StorageExplorer.zip"
Changing directory to /Users/st/Downloads/2021-11-16-19-19-44
Working dir: /Users/st/Downloads/2021-11-16-19-19-44
Downloading https://github.com/microsoft/AzureStorageExplorer/releases/download/v1.21.3/Mac_StorageExplorer.zip
Mac_StorageExplorer.zip
Redirecting to (maybe this can help us with version):

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   662  100   662    0     0   2259      0 --:--:-- --:--:-- --:--:--  2330
100  201M  100  201M    0     0  16.8M      0  0:00:11  0:00:11 --:--:-- 33.6M
archiveTempName: Mac_StorageExplorer.zip
archivePath: https://objects.githubusercontent.com/github-production-release-asset-2e65be/124597291/cb3958ea-ea6e-42a1-9539-08529efd31e6?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20211116%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20211116T181945Z&X-Amz-Expires=300&X-Amz-Signature=fcbf7f537daf3b448d132de443e3dad5036fd3610e0ffe326bf79dfda4e5f419&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=124597291&response-content-disposition=attachment%3B%20filename%3DMac_StorageExplorer.zip&response-content-type=application%2Foctet-stream
Calculated archiveName: Mac_StorageExplorer.zip
name: Mac_StorageExplorer
archiveExt: zip
identifier: macstorageexplorer
Compressed file found
App found: /Users/st/Downloads/2021-11-16-19-19-44/Microsoft Azure Storage Explorer.app
Application investigation.
Team ID found for app: UBF8T346G9
https://github.com/microsoft/AzureStorageExplorer/releases/download/v1.21.3/Mac_StorageExplorer.zip

**********

Found GitHub path
Github place: microsoft AzureStorageExplorer
Latest URL on github: https://github.com/microsoft/AzureStorageExplorer/releases/download/v1.21.3/Mac_StorageExplorer.zip 
Latest version: 1.21.3
GitHub calculated URL matches entered URL.

**********

Labels should be named in small caps, numbers 0-9, “-”, and “_”. No other characters allowed.

macstorageexplorer)
    name="Microsoft Azure Storage Explorer"
    type="zip"
    downloadURL="$(downloadURLFromGit microsoft AzureStorageExplorer)"
    appNewVersion="$(versionFromGit microsoft AzureStorageExplorer)"
    expectedTeamID="UBF8T346G9"
    ;;

Label converted to GitHub label without errors.
Details can be seen above.

Above should be saved in a file with exact same name as label, and given extension “.sh”.
Put this file in folder “fragments/labels”.
```

This worked out as the downloaded archive ended in “zip” and there was only one archive with that extension in this release.

## Archive name on github

In the current case, it could be argued that zip would be enough to find the Mac version among the Linux and Windows versions, as that is the only zip archive, but we can make certain to hit the Mac release by using the variable `archiveName` by specifying the name of the archive.

But take a moment and go through the most recent releases and verify for yourself that the Mac archive have been named like this in these releases. If that was not the case, we might simply just let installomator detect the zip archive, and hope no other platform version would be relased in a type like that.

But we can safely use this (I expect, but we have no idea if something changes in the future):
`archiveName="Mac_StorageExplorer.zip"`

Download that latest version now, and expand that so we can detect it's name (without the .app extension):
`name="Microsoft Azure Storage Explorer"` 

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

# Other GitHub titles

## Marathon trilogy

For old Mac fans, The Marathon trilogy has been released, also on GitHub.

For “Marathon 2” the archive is here: [https://github.com/Aleph-One-Marathon/alephone/releases/download/release-20210408/Marathon2-20210408-Mac.dmg](https://github.com/Aleph-One-Marathon/alephone/releases/download/release-20210408/Marathon2-20210408-Mac.dmg)

And `buildLables.sh` will do some calculations on that download here:
```
Found GitHub path
Github place: Aleph-One-Marathon alephone
Latest URL on github: https://github.com/Aleph-One-Marathon/alephone/releases/download/release-20210408/AlephOne-20210408-Mac.dmg 
Latest version: 20210408
Calculated GitHub URL almost identical, only this diff:
“release-20210408/Marathon2-20210408-Mac.dmg” and “release-20210408/AlephOne-20210408-Mac.dmg”
Could be version difference or difference in archiveName for a given release.
Testing for version difference.
Not a version problem.
Testing for difference in archiveName.
archiveName="Marathon2-[0-9.]*-Mac.dmg"
Latest URL on github: https://github.com/Aleph-One-Marathon/alephone/releases/download/release-20210408/Marathon2-20210408-Mac.dmg 
Latest version: 20210408
GitHub calculated URL matches entered URL.
```

And end up with this label:
```
marathon220210408mac)
    name="Marathon 2"
    type="dmg"
    archiveName="Marathon2-[0-9.]*-Mac.dmg"
    downloadURL="$(downloadURLFromGit Aleph-One-Marathon alephone)"
    appNewVersion="$(versionFromGit Aleph-One-Marathon alephone)"
    expectedTeamID="E8K89CXZE7"
    ;;
```

So only the label name needs to be renamed to `marathon2` and we are all set.

Actuallly all three titles are in the same “alephone” repository, but now it can calculate a working `archiveName`, that I would not change in this case as all titles end in “-Mac.dmg”.

# GitHub fun

__For fun__, try to go to the releases of the [Wally-label in Tutorial 3](https://github.com/zsa/wally/releases). The latest release is right now only a Linux version, and the latest Mac release is somewhere down the list. We cannot use the built in tools in Installomator for github in this situation, as they have not aligned the releases so that the Mac version will be released in each release, and Installomator would fail (the app is also versioned incorrectly, but that is another story to be read in that tutorial).

