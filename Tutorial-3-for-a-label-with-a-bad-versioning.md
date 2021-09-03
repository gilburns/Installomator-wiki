We try to illustrate the systematics in creating a label, and show the procedure, but it goes somewhat bad with this label. Maybe software developers will read this, and help with providing better web pages and versioning of apps. 

Example: Wally EZ Flash-software (dmg), and sed

## Start with buildLabel.sh

First we grab an URL from the web site, and give that to `buildLabel.sh`, like this:
```
% /buildLabel.sh https://configure.zsa.io/wally/osx
Changing directory to 2021-09-03-13-41-56
Working dir: ~/Downloads/2021-09-03-13-41-56
Downloading https://configure.zsa.io/wally/osx
Redirecting to (maybe this can help us with version):
location: https://github.com/zsa/wally/releases/download/2.1.0-osx/wally-osx-2.1.0.dmg
location: https://github-releases.githubusercontent.com/186514878/1ac0ef80-0969-11eb-83d9-6b11aca23861?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20210903%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210903T114217Z&X-Amz-Expires=300&X-Amz-Signature=12ad99b6285f10c6213a0651c3c67421666abb444be9cbbd2f377617410fe442&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=186514878&response-content-disposition=attachment%3B%20filename%3Dwally-osx-2.1.0.dmg&response-content-type=application%2Foctet-stream
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    91  100    91    0     0      9      0  0:00:10  0:00:09  0:00:01    23
  0     0    0     0    0     0      0      0 --:--:--  0:05:24 --:--:--     0
curl: (56) LibreSSL SSL_read: Connection reset by peer, errno 54
error downloading https://configure.zsa.io/wally/osx
st@Sren-ENVO-IT Downloads % /Users/st/Documents/GitHub/Installomator-Theile/buildLabel.sh https://configure.zsa.io/wally/osx
Changing directory to 2021-09-03-13-48-44
Working dir: /Users/st/Downloads/2021-09-03-13-48-44
Downloading https://configure.zsa.io/wally/osx
Redirecting to (maybe this can help us with version):
location: https://github.com/zsa/wally/releases/download/2.1.0-osx/wally-osx-2.1.0.dmg
location: https://github-releases.githubusercontent.com/186514878/1ac0ef80-0969-11eb-83d9-6b11aca23861?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20210903%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210903T114845Z&X-Amz-Expires=300&X-Amz-Signature=2aa88c6bb777ca56b76a3397476bde5a55465f69133aa92f892da766c4bfe412&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=186514878&response-content-disposition=attachment%3B%20filename%3Dwally-osx-2.1.0.dmg&response-content-type=application%2Foctet-stream
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    91  100    91    0     0    375      0 --:--:-- --:--:-- --:--:--   383
100   627  100   627    0     0   1312      0 --:--:-- --:--:-- --:--:--  1312
100 7637k  100 7637k    0     0   746k      0  0:00:10  0:00:10 --:--:--  773k
downloadOut:
wally-osx-2.1.0.dmg
https://github-releases.githubusercontent.com/186514878/1ac0ef80-0969-11eb-83d9-6b11aca23861?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20210903%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210903T114845Z&X-Amz-Expires=300&X-Amz-Signature=2aa88c6bb777ca56b76a3397476bde5a55465f69133aa92f892da766c4bfe412&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=186514878&response-content-disposition=attachment%3B%20filename%3Dwally-osx-2.1.0.dmg&response-content-type=application%2Foctet-stream
archiveTempName: wally-osx-2.1.0.dmg
archivePath: https://github-releases.githubusercontent.com/186514878/1ac0ef80-0969-11eb-83d9-6b11aca23861?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20210903%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210903T114845Z&X-Amz-Expires=300&X-Amz-Signature=2aa88c6bb777ca56b76a3397476bde5a55465f69133aa92f892da766c4bfe412&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=186514878&response-content-disposition=attachment%3B%20filename%3Dwally-osx-2.1.0.dmg&response-content-type=application%2Foctet-stream
archiveName: wally-osx-2.1.0.dmg
name: wally-osx-2.1.0
archiveExt: dmg
identifier: wallyosx210
Diskimage found
Mounting wally-osx-2.1.0.dmg
Mounted: /Volumes/Wally
Verifying: /Volumes/Wally/Wally.app
"disk4" ejected.

**********

Labels should be named in small caps, numbers 0-9, “-”, and “_”. No other characters allowed.

appNewVersion is often difficult to find. Can sometimes be found in the filename, sometimes as part of the download redirects, but also on a web page. See redirect and archivePath above if link contains information about this. That is a good place to start

wallyosx210)
    name="wally-osx-2.1.0"
    type="dmg"
    downloadURL="https://configure.zsa.io/wally/osx"
    appNewVersion=""
    expectedTeamID="V32BWKSNYH"
    appName="Wally.app"
    ;;

Above should be saved in a file with exact same name as label, and given extension “.sh”.
Put this file in folder “fragments/labels”.
```

This is actually being downloaded from GitHub, so maybe we can grab it from there instead.

It is here on github: https://github.com/zsa/wally/releases

But the relases are not done so we can just grab the latest release and use that on Mac, as that is for Linux. The versions do not follow each other.

I would probably stop here, and use `valuesfromarguments`, like this:
```
Installomator valuesfromarguments \
              name=Wally \
              type=dmg \
              downloadURL=https://configure.zsa.io/wally/osx \
              expectedTeamID=V32BWKSNYH \
              BLOCKING_PROCESS_ACTION=prompt_user \
              NOTIFY=all
```

If we were to insist on making a label we would have to clean up the above output, the label will look like this:
```
wallyezflash)
    name="Wally"
    type="dmg"
    downloadURL="https://configure.zsa.io/wally/osx"
    appNewVersion=""
    expectedTeamID="V32BWKSNYH"
    ;;
```

## Manually grab TeamID

In the priginal writing of this turorial, I did not get the TeamID out in the original run of `buildLabel.sh`, so I wrote how to manually find the TeamID, like this:
```
% spctl -a -vv /Volumes/Wally/Wally.app 
/Volumes/Wally/Wally.app: accepted
source=Notarized Developer ID
origin=Developer ID Application: ZSA Technology Labs Inc. (V32BWKSNYH)
```

Look at the lasty part of the last line in the parantheses. That's the TeamID: V32BWKSNYH

## Finding version from URL

To find the version, we can already notice in the URLs above that the version is part of the URL.

It is always the best to make sure that labels contain the `appNewVersion` variable, as we can then make sure to only ask the user to close the app, if we actually have an update for them. If it is not provided, Installomator will ask to close it, before going through with the update, even though it is actually not updated.

From looking at the output from `buildLabel.sh` we saw Location-lines with version number in them. Those can be used.

It is done with `curl -fsIL` and then enter the download URL, as that will write out headings (I) from the server of the file, and accept redirects (L) (with force and silent), like this:
`curl -fsIL https://configure.zsa.io/wally/osx`

You will get several fields out as text, and location can be the first to look at:
`location: https://github.com/zsa/wally/releases/download/2.1.0-osx/wally-osx-2.1.0.dmg`

So we have the version in the URL, all we need is to simply extract that:
`curl -fsIL "$downloadURL" | grep -i ^location | head -1 | sed -E 's/.*\/[a-zA-Z\-]*-([0-9.]*)\..*/\1/g'`

`grep -i` is finding lines with text not considering case, and `^` means beginning of line (some servers return Location, others location).
`head -1` gives us first line of output.
`sed -E` uses regular expressions to isolate the version number within () and only return that with \1, in a format 's/search/replace/g'

So I would add this line, if it matches the version in the app we downloaded:
`appNewVersion=$(curl -fsIL "$downloadURL" | grep -i ^location | head -1 | sed -E 's/.*\/[a-zA-Z\-]*-([0-9.]*)\..*/\1/g')`

## Finding version in app

So what version is the app we downloaded, we can see that with this command:
```
% defaults read /Volumes/Wally/Wally.app/Contents/Info.plist 
{
    CFBundleExecutable = Wally;
    CFBundleIconFile = Wally;
    CFBundleIdentifier = "com.zsa.wally";
    CFBundleName = Wally;
    CFBundlePackageType = APPL;
    CFBundleVersion = "2.0.0";
    CFShortVersionString = "2.0.0";
    NSHighResolutionCapable = 1;
}
```

We notice that it is using different names in this file as is custom to use for version, which is `CFBundleShortVersionString`.

So we can add `versionKey="CFBundleVersion"` to our label.

## Version does not match between URL app

The app is only version 2.0.0, but the URL gives 2.1.0. So none of this will match, and should not be used. I comment them out. Our result is then this:
```
wallyezflash)
     name="Wally"
     type="dmg"
     downloadURL="https://configure.zsa.io/wally/osx"
     #appNewVersion=$(curl -fsIL "${downloadURL}" | grep -i ^location | head -1 | sed -E 's/.*\/[a-zA-Z\-]*-([0-9.]*)\..*/\1/g')
     expectedTeamID="V32BWKSNYH"
     #versionKey="CFBundleVersion"
     ;;
```

Not sure I think this is a great label to include in our release, so if you need this, stick to `valuesfromarguments` above.
