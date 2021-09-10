Example: Authy Desktop, with grep and sed

So, someone at the Slack channel asked:
“Hello, Was anyone able to make Authy work with installomator?”

Well, this is what I did…

## I DuckDuckGo’ed “Authy”
(I don't google for stuff anymore…)

I got a link to this download page: [https://authy.com/download/](https://authy.com/download/), where I had to push a button to initiate the download.

So I right-clicke that button, and used "Inspect" in Safari. Here I could locate this URL for Mac:
- [https://electron.authy.com/download?channel=stable&arch=x64&platform=darwin&version=latest&product=authy](https://electron.authy.com/download?channel=stable&arch=x64&platform=darwin&version=latest&product=authy)

## Ready for buildLabel.sh

Time to use `buildLabel.sh` with this URL:
```
% buildLabel.sh "https://electron.authy.com/download?channel=stable&arch=x64&platform=darwin&version=latest&product=authy"
Changing directory to 2021-09-10-22-15-04
Working dir: 2021-09-10-22-15-04
Downloading https://electron.authy.com/download?channel=stable&arch=x64&platform=darwin&version=latest&product=authy
Redirecting to (maybe this can help us with version):

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    24  100    24    0     0     44      0 --:--:-- --:--:-- --:--:--    45
100 67.0M  100 67.0M    0     0  9044k      0  0:00:07  0:00:07 --:--:-- 11.1M
downloadOut:
download?channel=stable&arch=x64&platform=darwin&version=latest&product=authy
https://s3.amazonaws.com/authy-electron-repository-production/authy/stable/1.8.4/darwin/x64/Authy%20Desktop-1.8.4.dmg
archiveTempName: download?channel=stable&arch=x64&platform=darwin&version=latest&product=authy
archivePath: https://s3.amazonaws.com/authy-electron-repository-production/authy/stable/1.8.4/darwin/x64/Authy%20Desktop-1.8.4.dmg
archiveName: Authy%20Desktop-1.8.4.dmg
name: Authy%20Desktop-1.8.4
archiveExt: dmg
identifier: authydesktop184
Diskimage found
Mounting Authy%20Desktop-1.8.4.dmg
Mounted: /Volumes/Authy Desktop 1.8.4
Verifying: /Volumes/Authy Desktop 1.8.4/Authy Desktop.app
"disk4" ejected.

**********

Labels should be named in small caps, numbers 0-9, “-”, and “_”. No other characters allowed.

appNewVersion is often difficult to find. Can sometimes be found in the filename, sometimes as part of the download redirects, but also on a web page. See redirect and archivePath above if link contains information about this. That is a good place to start

authydesktop184)
    name="Authy%20Desktop-1.8.4"
    type="dmg"
    downloadURL="https://electron.authy.com/download?channel=stable&arch=x64&platform=darwin&version=latest&product=authy"
    appNewVersion=""
    expectedTeamID="9EVH78F4V4"
    appName="Authy Desktop.app"
    ;;

Above should be saved in a file with exact same name as label, and given extension “.sh”.
Put this file in folder “fragments/labels”.
```

## Version

We can see that the version is part of the file name. But it did not came out from the redirecting of the download link. I have not seen this behavior before.

So to demonstrate, this command to show the redirecting headers probably does not offer anything:
```
% curl -fsIL "https://electron.authy.com/download?channel=stable&arch=x64&platform=darwin&version=latest&product=authy"
HTTP/1.1 403 Forbidden
Date: Fri, 10 Sep 2021 20:31:32 GMT
Content-Type: application/json
Content-Length: 0
Connection: keep-alive
x-amzn-RequestId: 7754edcf-33ce-4dcb-a570-e2f85c88bde9
x-amzn-ErrorType: MissingAuthenticationTokenException
x-amz-apigw-id: FdsJqG4loAMFvcg=
X-Cache: Error from cloudfront
Via: 1.1 66fb345923f3acbd40f99fbda8e88694.cloudfront.net (CloudFront)
X-Amz-Cf-Pop: CPH50-C2
X-Amz-Cf-Id: qP7sC9sMg5TLX-lHD0DIlbvs2L_3lbVN7sqFlM4r66Wg5vAKXwaRdw==
CF-Cache-Status: DYNAMIC
Expect-CT: max-age=604800, report-uri="https://report-uri.cloudflare.com/cdn-cgi/beacon/expect-ct"
X-Content-Type-Options: nosniff
Server: cloudflare
CF-RAY: 68cb6b9fdbe4d88d-CPH
```
Yes, no version there.

So how did `buildLabel.sh` get the version out?

Well, with a combination of `curl` commands in the script, can we do this:
```
% curl -fL --output /dev/null -r 0-0 "https://electron.authy.com/download?channel=stable&arch=x64&platform=darwin&version=latest&product=authy" --remote-header-name --remote-name -w "%{filename_effective}\n%{url_effective}\n"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    24  100    24    0     0     64      0 --:--:-- --:--:-- --:--:--    65
100     1  100     1    0     0      1      0  0:00:01 --:--:--  0:00:01  1000
/dev/null
https://s3.amazonaws.com/authy-electron-repository-production/authy/stable/1.8.4/darwin/x64/Authy%20Desktop-1.8.4.dmg
```

So “filename_effective” did not contain anything, but “url_effective” did. So what about this command to get the URL:
```
% curl -sfL --output /dev/null -r 0-0 "https://electron.authy.com/download?channel=stable&arch=x64&platform=darwin&version=latest&product=authy" --remote-header-name --remote-name -w "%{url_effective}\n"
https://s3.amazonaws.com/authy-electron-repository-production/authy/stable/1.8.4/darwin/x64/Authy%20Desktop-1.8.4.dmg
```

Yes, so now we just needs to isolate the version:
```
% curl -sfL --output /dev/null -r 0-0 "https://electron.authy.com/download?channel=stable&arch=x64&platform=darwin&version=latest&product=authy" --remote-header-name --remote-name -w "%{url_effective}\n" | grep -o -E '([a-zA-Z0-9\_.%-]*)\.(dmg|pkg|zip|tbz)$' | sed -E 's/.*-([0-9.]*)\.dmg/\1/g'
1.8.4
```

`grep -o -E` means extended grep (with regular expression) only returning the found part, and the stuff in `''` is compied from the `buildLabel.sh` for matching the file name (with a `^` removed in the beginnning as our string does not begin with this).

`sed -E` also uses regular expression, and here I search for a specific pattern `.*-([0-9.]*)\.dmg`, but only returning part of it `[0-9.]*` as the `\1`.

## Cleaning up label

So now we can clean up the label, and shorten the versioning a bit (I had to mount the downloaded dmg, to be certain of the name of the app):
```
authydesktop)
    name="Authy Desktop"
    type="dmg"
    downloadURL="https://electron.authy.com/download?channel=stable&arch=x64&platform=darwin&version=latest&product=authy"
    appNewVersion="$(curl -sfL --output /dev/null -r 0-0 "${downloadURL}" --remote-header-name --remote-name -w "%{url_effective}\n" | grep -o -E '([a-zA-Z0-9\_.%-]*)\.(dmg|pkg|zip|tbz)$' | sed -E 's/.*-([0-9.]*)\.dmg/\1/g')"
    expectedTeamID="9EVH78F4V4"
    ;;
```

`appName` not needed as that was the same name as in `name` ( with .app appended).

This is only the Intel version, so have to hope for a universal version some day, but couldn't locate this in the URLs on the web page.
