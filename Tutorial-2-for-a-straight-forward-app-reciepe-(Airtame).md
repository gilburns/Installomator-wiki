Example: Airtame, with grep, cut, and sed commands
Download from: `https://airtame.com/download/`

## Isolating download link

Hoover the mouse over the download link, and notice that part of the URL is “`platform=Mac`.” Let's see if we can use that for something:

```
% curl -fs https://airtame.com/download/ | grep -i platform=mac
<a class="air-button air-button my-4 air-button--primary air-button--large" href="https://downloads-website.airtame.com/get.php?platform=mac&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830" data-air-test="download_for_mac">
<p><a href="https://downloads-website.airtame.com/get.php?platform=mac&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830">Mac</a></p>
<p><a href="https://downloads-website.airtame.com/get.php?platform=mac&amp;pkg=true&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830">Mac (PKG)</a></p>
```

`grep -i` means case insensitive search for lines with “`platform=mac`” in them.

That was a hit. I think I will prefer that pkg-installer, so I will choose this download:

```
% curl -fs https://airtame.com/download/ | grep -i platform=mac | grep -i pkg=true
<p><a href="https://downloads-website.airtame.com/get.php?platform=mac&amp;pkg=true&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830">Mac (PKG)</a></p>
```

The download link is hidden in a `href-part` of the HTML code, surrounded by “`"`”. We can use `grep` to make sure we have the `https` part, use `cut` to separate the “"”, like this:

```
% curl -fs https://airtame.com/download/ | grep -i platform=mac | grep -i pkg=true | grep -o -i -E "https.*" | cut -d '"' -f1
https://downloads-website.airtame.com/get.php?platform=mac&amp;pkg=true&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830
```

`grep -o -i -E` has `-o` for only returning matching part and `-E` for regular expression.
`cut -d` is setting a delimiter of `"` and returning the first part with `-f1`

So now we have the `downloadURL`. Let's but it through `buildLabel.sh` (with quotes around the URL as it has special characters):

```
% /Users/st/Documents/GitHub/Installomator/utils/buildLabel.sh "https://downloads-website.airtame.com/get.php?platform=mac&amp;pkg=true&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830"
Changing directory to /Users/st/Downloads
Downloading https://downloads-website.airtame.com/get.php?platform=mac&amp;pkg=true&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830
downloadOut: get.php?platform=mac&amp;pkg=true&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830
https://airtame-app.b-cdn.net/app/latest/mac/Airtame-4.2.1.dmg
archiveTempName: get.php?platform=mac&amp;pkg=true&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830
archivePath: https://airtame-app.b-cdn.net/app/latest/mac/Airtame-4.2.1.dmg
archiveName: Airtame-4.2.1.dmg
name: Airtame-4.2.1
archiveExt: dmg
identifier: airtame421
Diskimage found
Mounting Airtame-4.2.1.dmg
Mounted: /Volumes/Airtame 4.2.1
Verifying: /Volumes/Airtame 4.2.1/Airtame.app
"disk4" ejected.

**********

Labels should be named in small caps, numbers 0-9, “-”, and “_”. No other characters allowed.

appNewVersion is often difficult to find. Can sometimes be found in the filename, but also on a web page. See archivePath above if link contains information about this.

airtame421)
    name="Airtame-4.2.1"
    type="dmg"
    downloadURL="https://downloads-website.airtame.com/get.php?platform=mac&amp;pkg=true&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830"
    appNewVersion=""
    expectedTeamID="4TPSP88HN2"
    appName="Airtame.app"
    ;;

Above should be saved in a file with exact same name as label, and given extension “.sh”.
Put this file in folder “fragments/labels”.
```

Notice how we also get a version number. BUT also notice how we did not get a pkg installer, even though the donwload link kind of hintet at that. We should probably just use the first URL instead, and locate the download link like this:

```
% curl -fs https://airtame.com/download/ | grep -i platform=mac | head -1 | grep -o -i -E "https.*" | cut -d '"' -f1
https://downloads-website.airtame.com/get.php?platform=mac&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830
```

`head -1` gives the first line only in the previous result.

## Isolating the version number

It looks like the version number is part of the download URL in a redirect, so let try this command:

```
% curl -fsIL "https://downloads-website.airtame.com/get.php?platform=mac&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830" | grep -i ^location
location: https://airtame-app.b-cdn.net/app/latest/mac/Airtame-4.2.1.dmg
```

`curl -fsIL` means we want `force` and `silent`, `I` gives only headers (no data), and `L` follows redirects.
`grep -i` means we want case insesitive search for `location` in the beginning of the line with `^` (some servers return field names with capital letters, some do not).

So we did get the version number! Let's isolate that (using `sed` with regular expresion):
```
% curl -fsIL "https://downloads-website.airtame.com/get.php?platform=mac&amp;pkg=true&amp;_ga=2.103036160.1897251541.1572251943-2078298285.1570016830" | grep -i ^location | sed -E 's/.*\/[a-zA-Z]*-([0-9.]*)\..*/\1/g'
4.2.1
```

`sed -E` means we use `sed` with regular expression, and the structure of the command is `'s/search/replace/g'`. In the search part is `()` and with `\1` in replace, we only return what is in these paratheses. Before the parantheses is a dash `-`, before that som letters between a and z (both upper and lower case) `[a-zA-Z]`, before that a slash that needs to be escaped with `\/`. Before that it's `.*` witch will match anything. After paranthesis is an escaped dot `\.` as well as anything `.*`.

## Final label

So we can now clean up the final label with download link and version. In order to not repeat the download link command, iwe refer to it by it’s variable name. Like this:

```
airtame)
    name="Airtame"
    type="dmg"
    downloadURL="$(curl -fs https://airtame.com/download/ | grep -i platform=mac | grep -i pkg=true | grep -o -i -E "https.*" | cut -d '"' -f1)"
    appNewVersion="$(curl -fsIL "${downloadURL}" | grep -i ^location | sed -E 's/.*\/[a-zA-Z]*-([0-9.]*)\..*/\1/g')"
    expectedTeamID="4TPSP88HN2"
    ;;
```

If `appName` is equal to `name`, with only `.app` appended, than we don't need that, so that was removed. I also cleaned up the label name and the `name` variable.

I have verified this by opening the dmg that was downloaded, and looked at the app there. Both version and name match what we assumed.
