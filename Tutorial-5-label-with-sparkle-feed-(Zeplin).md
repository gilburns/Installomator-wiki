Example: Zeplin.app, with xpath

This label started out with a user reporting a problem with the output in `buildLabel.sh` when trying to make a label for this app. The person had already figured most of it out, but I wanted to fix the build-script, and then it turned out as a great example for an app with a XML Sparkle feed.

## `buildLabel.sh`

From the previous tutorials I had to refine the app extension detection in the script, then I got this smooth output:
```
% buildLabel.sh https://zpl.io/download-mac
Changing directory to ./2021-09-17-20-01-00
Working dir: ./2021-09-17-20-01-00
Downloading https://zpl.io/download-mac
Redirecting to (maybe this can help us with version):
location: https://appcenter-filemanagement-distrib5ede6f06e.azureedge.net/be87e8f9-ad94-4623-9efe-8f41d809bc98/Zeplin.app.zip?sv=2019-02-02&sr=c&sig=B0AK6kJ1JT4wPTLZs%2BGFbrY%2BlqD57D7osnalbwXacZY%3D&se=2021-09-18T01%3A34%3A08Z&sp=r
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   244  100   244    0     0    318      0 --:--:-- --:--:-- --:--:--   320
100 37.5M  100 37.5M    0     0  3692k      0  0:00:10  0:00:10 --:--:-- 1017k
archiveTempName: download-mac
archivePath: https://appcenter-filemanagement-distrib5ede6f06e.azureedge.net/be87e8f9-ad94-4623-9efe-8f41d809bc98/Zeplin.app.zip?sv=2019-02-02&sr=c&sig=B0AK6kJ1JT4wPTLZs%2BGFbrY%2BlqD57D7osnalbwXacZY%3D&se=2021-09-18T01%3A34%3A08Z&sp=r
Calculated archiveName: Zeplin.app.zip
name: Zeplin.app
archiveExt: zip
identifier: zeplinapp
Compressed file found
Verifying: /Users/st/Documents/GitHub/Installomator-Theile/2021-09-17-20-01-00/Zeplin.app

**********

Labels should be named in small caps, numbers 0-9, “-”, and “_”. No other characters allowed.

appNewVersion is often difficult to find. Can sometimes be found in the filename, sometimes as part of the download redirects, but also on a web page. See redirect and archivePath above if link contains information about this. That is a good place to start

zeplinapp)
    name="Zeplin.app"
    type="zip"
    downloadURL="https://zpl.io/download-mac"
    appNewVersion=""
    expectedTeamID="8U3Y4X5WDQ"
    appName="Zeplin.app"
    ;;

Above should be saved in a file with exact same name as label, and given extension “.sh”.
Put this file in folder “fragments/labels”.
```

So we just needs the version.

## Version

In some situations, the app contains the URL for which it will verify it's latest version, so it can update itself.
This URL can be located in the `Info.plist`-file, like here: `Zeplin.app/Contents/Info.plist`. In this file is the field `SUFeedURL` with this value:
`https://api.appcenter.ms/v0.1/public/sparkle/apps/8926efff-e734-b6d3-03d0-9f41d90c34fc`

This URL is a sparkle feed, so we can use this command to isolate the latest version:
```
% curl -fs "https://api.appcenter.ms/v0.1/public/sparkle/apps/8926efff-e734-b6d3-03d0-9f41d90c34fc" | xpath -e '(//rss/channel/item/enclosure/@sparkle:shortVersionString)[1]' 2>/dev/null | cut -d '"' -f 2
3.24
```

`xpath -e` is a command to go through xml files and locate data in those. Note that on Big Sur and Monterey we need parameter `-e`, but that is not used in Catalina. This difference is handled within Installomator, so for the final lavel we do not include the `-e`.

`'(//path/to/data/@field)[1]'` is the part that goes into the hierarchy of the xml, with `//rss/channel/item/enclosure` and here we want the field `sparkle:shortVersionString`. This alone would return all items in the xml with this field, and as the xml has the newest version at the top, we only need the first item `[1]`.

`cut` will set `"` as delimiter and return the 2. value.

## Final label

```
zeplin)
    name="Zeplin"
    type="zip"
    downloadURL="https://zpl.io/download-mac"
    appNewVersion="$(curl -fs "https://api.appcenter.ms/v0.1/public/sparkle/apps/8926efff-e734-b6d3-03d0-9f41d90c34fc" | xpath '(//rss/channel/item/enclosure/@sparkle:shortVersionString)[1]' 2>/dev/null | cut -d '"' -f 2)"
    expectedTeamID="8U3Y4X5WDQ"
    ;;
```

So I checked this with the `checkLabels.sh`-script, and that had to be fixed in regards to the special case of file name extensions in the URL as well.
