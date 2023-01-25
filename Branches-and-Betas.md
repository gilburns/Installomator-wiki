There are three important branches in the repo:

## main

This is the default branch.

The [`main`](https://github.com/Installomator/Installomator) branch is the one with the next version that _we are actively working on_ (the next minor version release).
 
It should only contain new application labels, updated application labels, and minor fixes to the code. Installomator is designed so that changes to labels should not affect the remaining code, so the main branch should be mostly safe, even when you get it from the main branch. Nevertheless, it may contain things that are in flux or not yet thoroughly tested.

When you are looking for the very latest patched application labels, this is the code you want to get and test.

## release

The [`release`](https://github.com/Installomator/Installomator/tree/release) branch will be sitting at the latest (non-beta) release version. This should be the safest code to use, but it may not contain the latest fixes to application labels.

You can also get the code for specific releases from archives that are attached to each release in [the Releases page](https://github.com/Installomator/Installomator/releases). There [are also git tags](https://github.com/Installomator/Installomator/tags) for each release (including the betas).

## dev

More significant code changes will be put together in the [`dev`](https://github.com/Installomator/Installomator/tree/dev) branch. There will be beta releases for these, but you can test with the code from the `dev` branch as well.

## Root Installomator.sh

The code in the `Installomator.sh` script and `Labels.txt` may not be in sync with updates in the labels in the `main` and `dev` branches. You want to run `./assemble.sh --script` to be sure you get the latest updates.

## Creating PRs

PRs should normally be created against the `main` branch. Be sure to pull or merge the latest changes from the main repository before you create your PR branch. If you know the changes in your PR will affect main code behavior outside of application labels, then you can branch off the `dev` branch, but it is also ok to branch off `main` and I will sort it out.
