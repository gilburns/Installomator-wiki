We try to illustrate the systematics in creating a label, and show the procedure, but it goes all bad with this label. Maybe software developers will read this, and help with providing better web pages and versioning of apps. But at least give it a read and see how bad it can be.

Example: Wally EZ Flash-software (dmg), and sed

## Start with buildLabel.sh

First we grab an URL from the web site, and give that to `buildLabel.sh`, like this:
```
% cd ~/Downloads
% /path/to/buildLabel.sh https://configure.zsa.io/wally/osx
Changing directory to /Users/st/Documents/GitHub/Installomator
Downloading https://configure.zsa.io/wally/osx
downloadOut: wally-osx-2.1.0.dmg
https://github-releases.githubusercontent.com/186514878/1ac0ef80-0969-11eb-83d9-6b11aca23861?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20210830%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210830T073520Z&X-Amz-Expires=300&X-Amz-Signature=de66741bd3e62e39e910aa373c25787dabe625e9a7acda39b63ff6b96752d3a8&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=186514878&response-content-disposition=attachment%3B%20filename%3Dwally-osx-2.1.0.dmg&response-content-type=application%2Foctet-stream
archiveTempName: wally-osx-2.1.0.dmg
archivePath: https://github-releases.githubusercontent.com/186514878/1ac0ef80-0969-11eb-83d9-6b11aca23861?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20210830%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210830T073520Z&X-Amz-Expires=300&X-Amz-Signature=de66741bd3e62e39e910aa373c25787dabe625e9a7acda39b63ff6b96752d3a8&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=186514878&response-content-disposition=attachment%3B%20filename%3Dwally-osx-2.1.0.dmg&response-content-type=application%2Foctet-stream
archiveName: 1ac0ef80-0969-11eb-83d9-6b11aca23861?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20210830%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210830T073520Z&X-Amz-Expires=300&X-Amz-Signature=de66741bd3e62e39e910aa373c25787dabe625e9a7acda39b63ff6b96752d3a8&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=186514878&response-content-disposition=attachment%3B%20filename%3Dwally-osx-2.1.0.dmg&response-content-type=application%2Foctet-stream
mv: rename wally-osx-2.1.0.dmg to 1ac0ef80-0969-11eb-83d9-6b11aca23861?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20210830%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210830T073520Z&X-Amz-Expires=300&X-Amz-Signature=de66741bd3e62e39e910aa373c25787dabe625e9a7acda39b63ff6b96752d3a8&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=186514878&response-content-disposition=attachment%3B%20filename%3Dwally-osx-2.1.0.dmg&response-content-type=application%2Foctet-stream: File name too long
name: 1ac0ef80-0969-11eb-83d9-6b11aca23861?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20210830%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210830T073520Z&X-Amz-Expires=300&X-Amz-Signature=de66741bd3e62e39e910aa373c25787dabe625e9a7acda39b63ff6b96752d3a8&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=186514878&response-content-disposition=attachment%3B%20filename%3Dwally-osx-2.1.0
archiveExt: dmg&response-content-type=application%2Foctet-stream
identifier: 1ac0ef80096911eb83d96b11aca23861?xamzalgorithm=aws4hmacsha256&xamzcredential=akiaiwnjyax4csveh53a20210830useast1s3aws4request&xamzdate=20210830t073520z&xamzexpires=300&xamzsignature=de66741bd3e62e39e910aa373c25787dabe625e9a7acda39b63ff6b96752d3a8&xamzsignedheaders=host&actorid=0&keyid=0&repoid=186514878&responsecontentdisposition=attachmentfilenamewallyosx210

**********

Labels should be named in small caps, numbers 0-9, “-”, and “_”. No other characters allowed.

appNewVersion is often difficult to find. Can sometimes be found in the filename, but also on a web page. See archivePath above if link contains information about this.

1ac0ef80096911eb83d96b11aca23861?xamzalgorithm=aws4hmacsha256&xamzcredential=akiaiwnjyax4csveh53a20210830useast1s3aws4request&xamzdate=20210830t073520z&xamzexpires=300&xamzsignature=de66741bd3e62e39e910aa373c25787dabe625e9a7acda39b63ff6b96752d3a8&xamzsignedheaders=host&actorid=0&keyid=0&repoid=186514878&responsecontentdisposition=attachmentfilenamewallyosx210)
    name="1ac0ef80-0969-11eb-83d9-6b11aca23861?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20210830%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210830T073520Z&X-Amz-Expires=300&X-Amz-Signature=de66741bd3e62e39e910aa373c25787dabe625e9a7acda39b63ff6b96752d3a8&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=186514878&response-content-disposition=attachment%3B%20filename%3Dwally-osx-2.1.0"
    type="dmg&response-content-type=application%2Foctet-stream"
    downloadURL="https://configure.zsa.io/wally/osx"
    appNewVersion=""
    expectedTeamID=""
    ;;

Above should be saved in a file with exact same name as label, and given extension “.sh”.
Put this file in folder “fragments/labels”.

%
```

This is the most un-clean output I have seen for any app. I notice that github is mentioned, so maybe it is much easier to grab the software from github. 

It is here on github: https://github.com/zsa/wally/releases

But the relases are not done so we can just grab the latest release and use that on Mac, as that is for Linux. The versions do not follow each other.

I would probably stop here, and use `valuesfromarguments`, like this:
```
Installomator valuesfromarguments \
              name=Wally \
              type=dmg \
              downloadURL=https://configure.zsa.io/wally/osx \
              expectedTeamID=V32BWKSNYH \
              BLOCKING_PROCESS_ACTION=prompt_user_loop \
              NOTIFY=all
```

If we were to insist on making a label we would have to clean up the above output, the label will look like this:
```
wallyezflash)
    name="Wally"
    type="dmg"
    downloadURL="https://configure.zsa.io/wally/osx"
    appNewVersion=""
    expectedTeamID=""
    ;;
```

## Manually grab TeamID

For some reason we did not get TeamID out, that must have been due to the script actually failing over the above output. We can find TeamID, by mounting the dmg manually, and run this from Terminal (I wrote `spctl -a -vv ` and dragged the app to Terminal:
```
% spctl -a -vv /Volumes/Wally/Wally.app 
/Volumes/Wally/Wally.app: accepted
source=Notarized Developer ID
origin=Developer ID Application: ZSA Technology Labs Inc. (V32BWKSNYH)
```

So at least we have `expectedTeamID="V32BWKSNYH"`, but what about the version.

## Finding version from URL

To find the version, we can already notice in the URLs above that the version is part of the URL.

It is always the best to make sure that labels contain the appNewVersion/variable, as we can then make sure to only ask the user to close the app, if we actually have an update for them. If it is not provided, Installomator will ask to close it, before going through with the updat, even though it is actually not updated.

The easiest way to start out is to do `curl -fsIL` and then enter the download URL, as that will write out headings (I) from the server of the file, and accept redirects (L) (with force and silent), like this:
`curl -fsIL https://configure.zsa.io/wally/osx`

You will get several fields out as text, and location can be the first to look at:
`location: https://github.com/zsa/wally/releases/download/2.1.0-osx/wally-osx-2.1.0.dmg`

So we have the version in the URL, all we need is to simply extract that:
`curl -fsIL "$downloadURL" | grep -i ^location | head -1 | sed -E 's/.*\/[a-zA-Z\-]*-([0-9.]*)\..*/\1/g'`

`grep -i` is finding lines with text not considering case, and `^` means beginning of line (some servers return Location, others location).
`head -1` gives us first line of output.
`sed -E` uses regular expressions to isolate the version number within () and only return that with \1, in a format 's/search/replace/g'

So I will add this line:
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