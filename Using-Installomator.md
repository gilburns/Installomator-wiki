There are several ways to run Installomator.

## Interactively in the command line

The script requires one argument.

The argument can be `version` or `longversion` which will print the script's version.

```
> ./Installomator.sh version
0.5.0
> ./Installomator.sh longversion
2021-03-28 10:04:16 longversion ################## Start Installomator v. 0.5.0
2021-03-28 10:04:16 longversion ################## longversion
2021-03-28 10:04:16 longversion Installomater: version 0.5.0 (2021-03-28)
```

Other than the version arguments, the argument can be any of the labels listed in the [`Labels.txt`](https://github.com/Installomator/Installomator/blob/main/Labels.txt) file. Each of the labels will download and install the latest version of the application, or suite of applications. Since the script will have to run the `installer` command or copy the application to the `/Applications` folder, it will have to be run as root.

```
> sudo ./Installomator.sh desktoppr DEBUG=0
```

*Note:* Jamf Pro provides the mount point, computer name, and user name as the first three arguments for policy scripts. Installomator will detect when it is running from Jamf Pro and ignore the first three arguments. For Jamf Pro policy scripts, you provide the application label as 'Parameter 4' and variable changes as subsequent parameters.

## Debug mode

There is a variable named `DEBUG` ([set here](https://github.com/Installomator/Installomator/blob/d4f70db24fa9c2bb41f805c1f83629465bfdbd89/Installomator.sh#L28)). When `DEBUG` is set to `1` (default) or `2`, no actions that would actually modify the current system are taken. This is useful for testing most of the actions in the script especially when building and testing new labels.

When the `DEBUG` variable is `1`, downloaded archives and extracted files will be written to the script's directory, rather than a temporary directory, which can make debugging easier. No actual installation is performed.

When `DEBUG` variable is `2`, the temporary folder is created and downloaded and extracted files goes to that folder, as if not in DEBUG mode. No installation performed, but blocking processes are checked, the app is reopened if closed, and the user is notified.

Debug mode 1 is useful to test the download and verification process without having to re-download and re-install an application or package on your system. Debug mode 2 is great for checking the workflow that checks and quit process and notifies the user.

**_Always remember_ to change the `DEBUG` variable to `0` when deploying.** 

The version of Installomator.sh in the installation pkg has `DEBUG=0`.

## Use Installomator with various MDM solutions

- [Jamf Pro](MDM:-Jamf-Pro)
- [Mosyle](MDM:-Mosyle-(Business,-Fuse,-and-Manager))
- [Addigy](MDM:-Addigy)

