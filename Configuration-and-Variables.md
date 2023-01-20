## Configuring the script

There are several default settings for certain behavior and notifications inside the script, but these can be customized when calling the script.

You can change the variables in the script directly. This means the default changes for your deployment for all instances where you use Installomator. You will have to remember to change the variables again when there is an update to Installomator that you deploy.

Alternatively, you can override variables when Installomator is run by providing additional arguments in the form `VARIABLE=new_value`

For example, when you want to override the `DEBUG` value to `0` (production) with out modifying the script, you can do:

```
sudo ./Installomator.sh <label> DEBUG=0
``` 

The application label has to be the first argument, followed by (optional) variable overrides. The order in which you override variables is irrelevant.

## Blocking Process actions

The `BLOCKING_PROCESS_ACTION` variable controls the behavior of the script when it finds a blocking process running. The default value is `tell_user`

- `ignore`: continue even when blocking processes are found.
- `silent_fail`: Exit script without prompt or installation.
- `prompt_user`: Show a user dialog for each blocking process found, user can choose "Quit and Update" or "Not Now". When "Quit and Update" is chosen, blocking process will be told to quit. Installomator will wait 30 seconds before checking again in case Save dialogs etc are being responded to. Installomator will abort if quitting after three tries does not succeed. "Not Now" will exit Installomator.
- `prompt_user_then_kill`: show a user dialog for each blocking process found, user can choose "Quit and Update" or "Not Now". When "Quit and Update" is chosen, blocking process will be terminated. Installomator will abort if terminating after two tries does not succeed. "Not Now" will exit Installomator.
- `prompt_user_loop`: Like prompt-user, but clicking "Not Now", will just wait an hour, and then it will ask again.
  WARNING! It might block the MDM agent on the machine, as the script will not exit, it will pause until the hour has passed, possibly blocking for other management actions in this time.
- `tell_user`: (Default) User will be showed a notification about the important update, but user is only allowed to Quit and Continue, and then we ask the app to quit. This is default.
- `tell_user_then_kill`: User will be showed a notification about the important update, but user is only allowed to Quit and Continue. If the quitting fails, the blocking processes will be terminated.
- `kill`: kill process without prompting or giving the user a chance to save.

If any process was closed, Installomator will try to open the app again, after the update process is done.

## Notification

The `NOTIFY` variable controls the notifications shown to the user. The default is `success`.

- `success`:   (default) notify the user after a successful install
- `silent`:    no notifications
- `all`:       all notifications (great for Self Service installation)

### Logo

The `LOGO` variable is used for the icon shown in dialog boxes. (But not notifications.)

- `appstore`:    Icon is Apple App Store (default)
- `jamf`:        JAMF Pro
- `mosyleb`:     Mosyle Business
- `mosylem`:     Mosyle Manager (Education)
- `addigy`:      Addigy

Alternatively a path to an icon file can be provided. E.g. `LOGO="/System/Applications/App\ Store.app/Contents/Resources/AppIcon.icns"` (spaces are escaped).

## App Store apps handling
The `IGNORE_APP_STORE_APPS` variable controls how Installomator deals with Apps that were previously installed from the App Store. The default is `no`.

Known bad example: Slack will 'forget' all settings when the non-App Store version is installed over the App Store version.

- `no`: when installed app is from App Store (which include VPP installed apps) it will not be touched, no matter it's version (default)
- `yes`: install over App Store (VPP) app, even if latest version 

## Owner of copied apps

The `SYSTEMOWNER` variable controls the owner for dmg and archive installations. The default is `0`.

The default behavior of setting the current user recreates the outcome of a manual drag-and-drop installation. This allows built-in update mechanisms, which run as the user, to update the app.

When you want to prevent built-in update mechanisms from updating the app, you will want to set `SYSTEMOWNER=1`.

However, this will prohibit the app from updating itself, but it will not prevent the app from attempting to do so, which may lead to errors and unnecessary downloads. In this case you _also_ need to configure the app in question to not perform updates. The configuration steps for this will be different for any given app and need to be done separately from Installomator.

- `0`: the current user will be owner of copied apps, just like if they installed it themselves. When there is no current user (i.e. system is at the login window), owner will be set to `root:wheel` (default)
- `1`: `root:wheel` will be set on the copied app. Useful for shared machines

## Install behavior (force installation)

For labels that are able to check the latest version of an app, downloads and installations will only be performed when the latest version (`appNewVersion`) differs from the installed version. The `INSTALL` variable can be used to force the installation in any case.

- ` `:           When not set, software is only installed if it is newer/different in version (default)
- `force`:       Install even if it’s the same version

## Re-opening of closed app

The `REOPEN` variable can be used to prevent the reopening of a closed app

- `yes`:   (default) app will be reopened if it was closed
- `no`:    app not reopened

## Values from Arguments

You can provide a configuration variable, such as `DEBUG` or `NOTIFY` as an argument in the form `VARIABLE=value`. For example:

```
./Installomator.sh desktoppr DEBUG=0 NOTIFY=silent
```

Providing variables this way will override any variables set in the script.

You can even provide _all_ the variables necessary for download and installation. Of course, without a label the argument parsing will fail, there is a special label `valuesfromarguments` which only checks if the four required values are present:

```
./Installomator.sh valuesfromarguments name=desktoppr type=pkg downloadURL=https://github.com/scriptingosx/desktoppr/releases/download/v0.3/desktoppr-0.3.pkg expectedTeamID=JME5BW3F3R 
```

The label has to be the first argument. The order of the variables is not relevant.

Providing all the variables this way might be useful for certain downloads that have a customized URL for each vendor/customer (like customized TeamView or Watchman Monitoring) or are local downloads.

