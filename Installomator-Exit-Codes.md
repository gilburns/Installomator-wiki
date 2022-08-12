Installomator has a number of exit codes when something goes wrong:

`0`: Success. Everything went well, or at least as expected

`1`: Unknown label

`1`: macOS Version too old

`1`: Active display sleep assertion detected

`1`: Error changing directory to tmp dir

`2`: Download Error

`3`: Error while mounting disk image

`3`: Error while accessing disk image mount point

`3`: Error while running the CLIInstaller

`4`: Error verifying download

`5`: Team IDs do not match

`6`: Not running as root

`6`: Minimum OS version of the app is higher than current OS

`7`: Error while copying the app

`8`: Could not find app to install

`9`: Error installing pkg

`9`: Could not get git repo for given user and repo name

`10`: User aborted update

`11`: Cloud not quit/kill all blocking processes

`12`: Blocking process found and `silent_fail` is set

`20`: Could not find pkg in dmg

`20`: Could not find pkg in zip

`20`: Could not find dmg in zip

`77`: No Download URL Set, this is an update only application and the updater failed

`99`: Unknown type

