(With “beginner”, it is still necessary to know how the Terminal is working and understand some basic shell scripting, but we hope it is still useful. Armin Briegel have created great articles for learning to script: [Scripting macOS](https://scriptingosx.com/2021/07/scripting-macos-part-1-first-script/) )

Example: ZoomRooms (pkg), and cut

From this web page: https://zoom.us/download
We find this download URL: https://zoom.us/client/latest/ZoomRooms.pkg

## Start with buildLabels.sh
We can use `buildLabel.sh` to start figuring out the label:

```
% buildLabel.sh https://zoom.us/client/latest/ZoomRooms.pkg
Changing directory to 2021-09-03-13-14-52
Working dir: ~/Downloads/2021-09-03-13-14-52
Downloading https://zoom.us/client/latest/ZoomRooms.pkg
Redirecting to (maybe this can help us with version):
location: https://cdn.zoom.us/prod/5.7.3865.0810/ZoomRooms.pkg
 % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                Dload  Upload   Total   Spent    Left  Speed
 0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100  342M  100  342M    0     0  3765k      0  0:01:33  0:01:33 --:--:-- 2742k
downloadOut:
ZoomRooms.pkg
https://cdn.zoom.us/prod/5.7.3865.0810/ZoomRooms.pkg
archiveTempName: ZoomRooms.pkg
archivePath: https://cdn.zoom.us/prod/5.7.3865.0810/ZoomRooms.pkg
archiveName: ZoomRooms.pkg
name: ZoomRooms
archiveExt: pkg
identifier: zoomrooms
Package found
For PKGs it's advised to find packageID for version checking
<pkg-ref id="us.zoom.pkg.zp" version="5.7.3865.0810" onConclusion="none" installKBytes="800263">#zp.pkg</pkg-ref>
us.zoom.pkg.zp
Above is the possible packageIDs that can be used, and the correct one is probably one of those with a version number. More investigation might be needed to figure out correct packageID if several are displayed.

**********

Labels should be named in small caps, numbers 0-9, “-”, and “_”. No other characters allowed.

appNewVersion is often difficult to find. Can sometimes be found in the filename, sometimes as part of the download redirects, but also on a web page. See redirect and archivePath above if link contains information about this. That is a good place to start

zoomrooms)
   name="ZoomRooms"
   type="pkg"
   packageID="us.zoom.pkg.zp"
   downloadURL="https://zoom.us/client/latest/ZoomRooms.pkg"
   appNewVersion=""
   expectedTeamID="BJ4HAAB9B3"
   ;;

Above should be saved in a file with exact same name as label, and given extension “.sh”.
Put this file in folder “fragments/labels”.
```

Please note the label type (`pkg`), and that our result contains `packageID`. The result could have been several IDs, and then we would have to figure out which one is the most likely to be used, like which one has the correct version.

## What about the version of the software?

Often, web pages offer a generic URL, that will be redirected to a versioned download URL. That can be seen from the above “Location”-lines. 

Here we run `curl -fsIL` and then enter the download URL, as that will write out headings (I) from the server of the file, and accept redirects (L) (with force and silent), like this::
`curl -fsIL https://zoom.us/client/latest/ZoomRooms.pkg`

The result will be a lot of lines with the various heading fields displayed, and if more redirects is occuring, then more lines will be added. Often we want to look at the field "location", and some servers will name this field with capital L, other just in small caps, so to return the location fields only, we use `grep -i ^location` to filter for these lines (“`-i`” for case insensitive and “`^`” to have “location” in the beginning of the line):

```
% curl -fsIL https://zoom.us/client/latest/ZoomRooms.pkg | grep -i ^location
location: https://cdn.zoom.us/prod/5.7.3865.0810/ZoomRooms.pkg
```

To isolate the version from this URL, we can use `cut` to separate the string into sections separated by “/” and give the 5. section as a result:

```
% curl -fsIL https://zoom.us/client/latest/ZoomRooms.pkg | grep -i ^location | cut -d "/" -f5
5.7.3865.0810
```

We can see that this version matches with the version we got out when we initially ran `buildLabel.sh`.

## Do other processes run together with our label

Some applications have helper processes running, to make sure that those are quit before we update, the variable `blockingProcesses` can be used.

In this case both “Zoom Rooms” and “ZoomPresence” can be running, so we add this line:
```
blockingProcesses=( "Zoom Rooms" "ZoomPresence" )
```

## Resulting label

In the end we can put it all together, and get this. Please not that our `curl`-command above was used on the `downloadURL`, so we would rather refer to the variable than repeat the URL manually:

```
zoomrooms)
    name="ZoomRooms"
    type="pkg"
    packageID="us.zoom.pkg.zp"
    downloadURL="https://zoom.us/client/latest/ZoomRooms.pkg"
    appNewVersion="$(curl -fsIL ${downloadURL} | grep -i ^location | cut -d "/" -f5)"
    expectedTeamID="BJ4HAAB9B3"
    blockingProcesses=( "Zoom Rooms" "ZoomPresence" )
    ;;
```