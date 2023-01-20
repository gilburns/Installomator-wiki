## Frequently Asked Questions

### What if the latest version of the app is already installed?

It depends on the label. Some labels can determine the latest version from the vendor's website or other resource. In that case, Installomator will see the latest version is already installed and do nothing. Labels that have a `appNewVersion` variable can do this. 

- Labels without `appNewVersion` will re-download and re-install the latest over the existing installation.
- Labels with `appNewVersion` will only download and install the app if the version is different than the one installed.
- Labels that can use update tool will use that for the update (if the version is different)

### Why don't you just use `autopkg install`?

Short answer: `autopkg` is not designed or suited for this kind of workflow

Long answer:

The motivation to not re-invent the wheel and use and existing tool is understandable. However, `autopkg` was not designed with this use case in mind and has a few significant downsides.

First, you would have to deploy and manage autopkg on all the clients. But to do its work, `autopkg` requires recipes. So, you have to install, and update the recipe repos on the client, as well. For security reasons, you _really_ should only run trusted recipes, so you need to install and update your personal recipe overrides as well.

The recipes you use are probably spread across multiple community provided recipe-repos, so we have `autopkg` itself, several recipe-repos, and your overrides that we need to manage, each of which may need to be updated at any time.

The community recipe-repos contain several recipes for different applications. When you add a recipe-repo for an app you want, you will also install all the other recipes from that repo.

The `autopkg install` does _not_ require root or even administrative privileges. _Any_ user (even standard users) on the system can now install any of the random recipes that came with the community repos.

To prevent users installing random apps from the community repos, you can curate your own recipe-repo from the community repos and push that to the clients. At this point, you are managing autopkg, your curated repo, your recipe overrides on the clients and handling the additional work of curating and updating your recipe-repo and the overrides.

In addition, a really savvy user (or a malicious attacker) could build their own recipe and run it using the pre-installed `autopkg` you installed.

And then consider what your CISO department (if you have one) would say about the `autopkgserver` and `autopkginstalld` daemons running on all the clients...

At this point it would be easier to use AutoPkg the way it was intended: on a single admin Mac, and let it upload the pkgs to your management system, which deploys them. Each tool is doing what it is designed for.

Please don't misunderstand this as me saying that AutoPkg is a bad or poorly designed tool. AutoPkg is amazing, powerful, and useful. The [Scripting OS X recipe-repo](https://github.com/autopkg/scriptingosx-recipes) is one of the older repos. AutoPkg is valuable tool to help admins with many apps that cannot be automated with tools like Installomator, and with deployment strategies that require more control.

But it is not suited as a client install automation tool.

### Why don't you just use brew or MacPorts?

Read the explanation for `autopkg`, pretty much the same applies for `brew`, i.e. while it is useful on a single Mac, it is a un-manageable mess when you think about deploying and managing on a fleet of computers.
