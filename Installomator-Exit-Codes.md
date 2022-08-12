Installomator has a number of exit codes when something goes wrong:

`0`: Success. Everything went well, or at least as expected

`1`: Unknown label

`2`: Download Error

`3`: Error while mounting disk image or while accessing disk image mount point

`4`: Error verifying download

`5`: Team IDs do not match

`6`: Not running as root

`7`: Error while copying the app

`8`: Could not find app to install

`9`: Error installing pkg

`10`: User aborted update

`11`: Cloud not quit/kill all blocking processes

`12`: Blocking process found and `silent_fail` is set

`13`: Error changing directory to tmp dir (used to be `1`)

`14`: Could not get git repo for given user and repo name (used to be `9`)

`15`: Minimum OS version of the app is higher than current OS (used to be `6`)

`16`: Error while running the CLIInstaller (used to be `3`)

`20`: Could not find pkg in dmg

`21`: Could not find pkg in zip (used to be `20`)

`22`: Could not find dmg in zip (used to be `20`)

`23`: App previously installed from App Store, and we respect that (used to be `1`)

`24`: Active display sleep assertion detected (used to be `1`)

`77`: No Download URL Set, this is an update only application and the updater failed

`98`: Installomater requires at least macOS Mojave

`99`: Unknown type

