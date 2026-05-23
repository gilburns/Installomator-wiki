Installomator provides a set of helper functions in `fragments/functions.sh` that label authors can call directly in their label definitions. These functions handle common tasks such as fetching download URLs and version strings from GitHub releases, querying XML feeds, and parsing JSON responses.

The functions described on this page are the **public API for label authors**. All other functions in `functions.sh` are used internally by Installomator and should not be called directly from a label.

> **Note on example formatting:** Some examples below split long variable assignments across multiple lines using `\` continuation for readability. In actual label files, assignments like `downloadURL=` and `appNewVersion=` must each be written as a single unbroken line.

---

## Table of Contents

- [`downloadURLFromGit`](#downloadurlfromgit)
- [`versionFromGit`](#versionfromgit)
- [`xpath`](#xpath)
- [`getJSONValue`](#getjsonvalue)

---

## `downloadURLFromGit`

Fetches the browser download URL for the latest release asset from a public GitHub repository.

### Signature

```sh
downloadURLFromGit <gitusername> <gitreponame>
```

### Parameters

| Parameter | Description |
|-----------|-------------|
| `gitusername` | The GitHub account or organization name (e.g., `"scriptingosx"`) |
| `gitreponame` | The repository name (e.g., `"desktoppr"`) |

### Behavior

The function determines the expected file extension from the label's `type` variable:

| `type` value | Extension searched for |
|---|---|
| `dmg` | `.dmg` |
| `pkg` | `.pkg` |
| `zip` | `.zip` |
| `pkgInDmg` | `.dmg` |
| `pkgInZip` | `.zip` |

It first queries the GitHub Releases API. If no matching asset is found there, it falls back to scraping the repository's `releases/latest` expanded assets page.

If the label sets the `archiveName` variable before calling this function, the value is used as a **substring match** against asset filenames instead of the file extension. This is useful when a release contains multiple platform-specific assets (e.g., `arm64` vs `x86_64`). `archiveName` may be a regex pattern.

On success the function **prints the URL to stdout** and returns `0`. If no URL can be determined, it calls `cleanupAndExit 14` and aborts the installation.

### Label Variable Dependencies

| Variable | Required | Notes |
|---|---|---|
| `type` | Yes | Controls which file extension is searched for |
| `archiveName` | No | Overrides extension matching with a specific filename or pattern |

### Examples

#### `desktoppr` — simple `pkg` release

```sh
# fragments/labels/desktoppr.sh
desktoppr)
    name="desktoppr"
    type="pkg"
    packageID="com.scriptingosx.desktoppr"
    downloadURL=$(downloadURLFromGit "scriptingosx" "desktoppr")
    appNewVersion=$(versionFromGit "scriptingosx" "desktoppr")
    expectedTeamID="JME5BW3F3R"
    blockingProcesses=( NONE )
    ;;
```

#### `utm` — simple `dmg` release

```sh
# fragments/labels/utm.sh
utm)
    name="UTM"
    type="dmg"
    downloadURL=$(downloadURLFromGit utmapp UTM)
    appNewVersion=$(versionFromGit utmapp UTM)
    expectedTeamID="WDNLXAD4W8"
    ;;
```

#### `jasp` — architecture-specific asset via `archiveName`

When a GitHub release publishes separate assets for Apple Silicon and Intel, set `archiveName` to a regex pattern before calling the function. The function will match against asset names rather than file extension.

```sh
# fragments/labels/jasp.sh
jasp)
    name="JASP"
    type="dmg"
    if [[ $(arch) == "arm64" ]]; then
        archiveName="JASP-[0-9.]*-macOS-arm64.dmg"
    elif [[ $(arch) == "i386" ]]; then
        archiveName="JASP-[0-9.]*-macOS-x86_64.dmg"
    fi
    downloadURL=$(downloadURLFromGit jasp-stats jasp-desktop)
    appCustomVersion(){ /usr/bin/defaults read "/Applications/JASP.app/Contents/Info.plist" "CFBundleVersion" | sed 's/..$//'; }
    appNewVersion=$(versionFromGit jasp-stats jasp-desktop)
    expectedTeamID="AWJJ3YVK9B"
    ;;
```

---

## `versionFromGit`

Fetches the version string of the latest release from a public GitHub repository.

### Signature

```sh
versionFromGit <gitusername> <gitreponame>
```

### Parameters

| Parameter | Description |
|-----------|-------------|
| `gitusername` | The GitHub account or organization name |
| `gitreponame` | The repository name |

### Behavior

The function follows the HTTP redirect from `https://github.com/<user>/<repo>/releases/latest` and extracts the version from the final path component (the tag name). All non-numeric, non-period characters are stripped, so tag names like `v1.2.3` or `release-1.2.3` both yield `1.2.3`.

On success the function **prints the version string to stdout** and returns `0`. If the version cannot be determined, it logs a `WARN`-level message and sets `appNewVersion` to an empty string (the installation will still proceed).

> **Note:** `versionFromGit` is typically paired with `downloadURLFromGit` in the same label so both values are retrieved from the same repository.

### Examples

The examples for `versionFromGit` are the same labels shown under [`downloadURLFromGit`](#downloadurlfromgit) above. Both functions are almost always called together:

```sh
downloadURL=$(downloadURLFromGit "scriptingosx" "desktoppr")
appNewVersion=$(versionFromGit "scriptingosx" "desktoppr")
```

---

## `xpath`

A compatibility wrapper around `/usr/bin/xpath` that transparently handles the breaking interface change Apple introduced in macOS Big Sur.

### Signature

```sh
xpath <expression> [file]
```

Standard input is forwarded to the underlying `xpath` binary, so the typical usage is to pipe a `curl` response into it.

### Parameters

| Parameter | Description |
|-----------|-------------|
| `expression` | An XPath expression string |
| `file` | Optional: path to an XML file (otherwise reads from stdin) |

### Behavior

On macOS Catalina and earlier (build ≤ `20A`), `/usr/bin/xpath` is called without the `-e` flag. On macOS Big Sur and later, the `-e` flag is required and is added automatically. Label authors can call `xpath` without worrying about which macOS version is running.

In both cases the `-q` (quiet) flag is used to suppress extra output from the binary.

### Common Patterns

XPath is most often used to parse **Sparkle appcast** RSS feeds, which many Mac apps publish for their built-in updater. Key XPath idioms used in labels:

| Pattern | What it selects |
|---|---|
| `(//rss/channel/item/enclosure/@url)[1]` | The `url` attribute of the first `<enclosure>` element |
| `(//rss/channel/item/enclosure/@sparkle:shortVersionString)[1]` | The human-readable version from the first enclosure |
| `[last()]` | The *last* item in the node set (some feeds list oldest-first) |
| `string(//installer-gui-script/pkg-ref[@id='...']/@version)` | Version from a flat pkg's Distribution XML |

The result of `xpath` typically includes the attribute name (`url="..."`) — use `cut -d '"' -f 2` to extract just the value.

### Examples

#### `omniplan4` — download URL from an appcast feed

```sh
# fragments/labels/omniplan4.sh
omniplan4)
    name="OmniPlan"
    type="dmg"
    downloadURL=$(curl -fs "https://update.omnigroup.com/appcast/com.omnigroup.OmniPlan4" \
        | xpath '(//rss/channel/item/enclosure/@url)[1]' 2>/dev/null \
        | head -1 | cut -d '"' -f 2)
    appNewVersion=$(echo "${downloadURL}" | sed -E 's/.*\/[a-zA-Z]*-([0-9.]*)\..*/\1/g')
    expectedTeamID="34YW5XSRB7"
    ;;
```

#### `typora` — both download URL and version from the same feed

```sh
# fragments/labels/typora.sh
typora)
    name="Typora"
    type="dmg"
    downloadURL=$(curl -fs "https://www.typora.io/download/dev_update.xml" \
        | xpath '(//rss/channel/item/enclosure/@url)[1]' 2>/dev/null \
        | cut -d '"' -f 2)
    appNewVersion=$(curl -fs "https://www.typora.io/download/dev_update.xml" \
        | xpath '(//rss/channel/item/enclosure/@sparkle:shortVersionString)[1]' 2>/dev/null \
        | cut -d '"' -f 2)
    expectedTeamID="9HWK5273G4"
    ;;
```

#### `latexit` — selecting the last item in the feed

```sh
# fragments/labels/latexit.sh
latexit)
    name="LaTeXiT"
    type="dmg"
    downloadURL="$(curl -fs "https://pierre.chachatelier.fr/latexit/downloads/latexit-sparkle-en.rss" \
        | xpath '(//rss/channel/item/enclosure/@url)[last()]' 2>/dev/null \
        | cut -d '"' -f 2)"
    appNewVersion="$(curl -fs "https://pierre.chachatelier.fr/latexit/downloads/latexit-sparkle-en.rss" \
        | xpath '(//rss/channel/item/enclosure/@sparkle:shortVersionString)[last()]' 2>/dev/null \
        | cut -d '"' -f 2)"
    expectedTeamID="7SFX84GNR7"
    ;;
```

---

## `getJSONValue`

Parses a JSON string or file and returns the value at a given key path, using JavaScriptCore via `osascript`.

### Signature

```sh
getJSONValue <json-or-filepath> <keypath>
```

### Parameters

| Parameter | Description |
|-----------|-------------|
| `json-or-filepath` | A JSON string **or** an absolute path to a JSON file (tested up to 1 GB strings and 2 GB files) |
| `keypath` | A key path in dot or bracket notation (e.g., `version`, `[0].url`, `Automatic.Version`) |

### Behavior

The function pipes its first argument into an `osascript` JavaScript snippet that calls `JSON.parse()` and navigates to the requested key path. If the resolved value is a JavaScript **object or array**, it is returned as pretty-printed JSON. Scalar values (strings, numbers, booleans) are returned as-is.

The function handles both direct JSON strings and file paths transparently — if the input string is a path to an existing file, the file's contents are read and parsed instead.

> **Credit:** This function was contributed by @Pico via the [Mac Admins Slack](https://macadmins.org).

### Key Path Notation

| Notation | Example | What it accesses |
|---|---|---|
| Dot | `Automatic.Version` | Nested object key `Version` inside object `Automatic` |
| Bracket | `[0].version` | Key `version` of the first element of an array |
| Mixed | `notes[0].version` | Key `version` of the first element of array `notes` |

### Examples

#### `googlechrome` — array bracket notation

```sh
# fragments/labels/googlechrome.sh
googlechrome)
    name="Google Chrome"
    type="dmg"
    downloadURL="https://dl.google.com/chrome/mac/universal/stable/GGRO/googlechrome.dmg"
    appNewVersion=$(getJSONValue \
        "$(curl -s "https://chromiumdash.appspot.com/fetch_releases?platform=Mac&channel=Stable&num=1")" \
        "[0].version")
    expectedTeamID="EQHXZ8M8AV"
    ;;
```

The endpoint returns a JSON array; `[0].version` navigates to the `version` key of the first element.

#### `elgatocamerahub` — caching a single request, dot notation

Fetching the JSON once and reusing it for both `appNewVersion` and `downloadURL` avoids a duplicate network request.

```sh
# fragments/labels/elgatocamerahub.sh
elgatocamerahub)
    name="Elgato Camera Hub"
    type="pkg"
    elgatoJSON=$(curl -fsSL "https://gc-updates.elgato.com/mac/echm-update/final/app-version-check.json")
    appNewVersion=$(getJSONValue "$elgatoJSON" "Automatic.Version")
    downloadURL=$(getJSONValue "$elgatoJSON" "Automatic.fileURL")
    appCustomVersion() {
        version=$(defaults read "/Applications/Camera Hub.app/Contents/Info.plist" CFBundleShortVersionString 2>/dev/null)
        build=$(defaults read "/Applications/Camera Hub.app/Contents/Info.plist" CFBundleVersion 2>/dev/null)
        echo "${version}.${build}"
    }
    expectedTeamID="Y93VXCB8Q5"
    blockingProcesses=( "Camera Hub" )
    ;;
```

#### `postman` — mixed notation (array inside named key)

```sh
# fragments/labels/postman.sh
postman)
    name="Postman"
    type="zip"
    curlOptions=( -H "accept-encoding: gzip, deflate, br" )
    if [[ $(arch) == "arm64" ]]; then
        downloadURL="https://dl.pstmn.io/download/latest/osx_arm64"
    elif [[ $(arch) == "i386" ]]; then
        downloadURL="https://dl.pstmn.io/download/latest/osx_64"
    fi
    appNewVersion=$(getJSONValue \
        "$(curl -fsL 'https://mkt.cdn.postman.com/www-next/release-notes/app-release-notes.json')" \
        'notes[0].version')
    expectedTeamID="H7H8Q7M5CK"
    ;;
```

The key path `notes[0].version` navigates into the `notes` array and retrieves the `version` key from the first (most recent) entry.

---

## Choosing the Right Function

| Situation | Recommended function |
|---|---|
| App releases on GitHub | `downloadURLFromGit` + `versionFromGit` |
| GitHub with multiple platform assets | `downloadURLFromGit` + `archiveName` |
| App publishes a Sparkle appcast | `xpath` on the RSS feed |
| Vendor provides a JSON update manifest | `getJSONValue` on the manifest |
| Vendor JSON is reused for both URL and version | Fetch into a variable, call `getJSONValue` twice |
