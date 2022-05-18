This is the first quick version of these instructions.

## Create GitHub account

You can create an account on GitHub and download the [GitHub Desktop](https://desktop.github.com) application to your Mac. Then you can use the green Open-button to open Installomator in your Desktop app:

![GitHub Desktop app](img/GitHub_Desktop_app.png)

## Create a branch for your label or other changes

In order to make changes you need to create a new branch. In GitHub Desktop go to the Branch “main” and create a new branch.

![GitHub Desktop app branch](img/GitHub_Desktop_app_branch.png)

## Make changes

__Remember to not change the Installomator.sh script, but the add or modify files in the `fragments` folder or sub-folders!__

Now your branch can be edited and new label files can be edited or other changes/improvements can be made.

Different Text editors can be used for this, but Xcode, BBEdit, or Sublime Text are great solutions.

Testing Installomator is done with the `assamble.sh` script:
```
Installomator/utils/assemble.sh label
```

If you want to test it on your system, run like this:
```
sudo Installomator/utils/assemble.sh label DEBUG=0
```

Of course other variables can be used on these commands, as well.

## Commit changes

GitHub Desktop lets you commit changes to your branch.

![GitHub Desktop commit changes](img/GitHub_Desktop_app_changes.png)

__Remember to document your changes before you commit.__ Then we can see what has been done.

A Terminal output of the label running should be added as a comment to the pull request.

## Create Pull Request (PR)

After changes are committed, you can create a Pull Request (PR).

![GitHub Desktop PR](img/GitHub_Desktop_app_PR.png)

You will be taken to the GitHub web page to create the PR, and then it will become public in our list of PRs.

__We thank you for you contribution to the project!__
