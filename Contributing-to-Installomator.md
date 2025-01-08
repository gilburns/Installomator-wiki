## Overview
Please note, that if you are contributing to this project with new labels or other suggestions in PRs, please put your changes in the files below fragments folder. __DO NOT edit the full `Installomator.sh` script.__

The full script is now a build of the fragments, and will be overwritten. See the README.md file in the utils directory for detailed instructions.

An [editorconfig file](https://github.com/Installomator/Installomator/blob/main/.editorconfig) is included at the root of the Installomator repo. As of January 2025, it is required to use it when submitting PRs. An editorconfig file defines settings like tabs vs spaces, how many spaces for a tab, and how to treat the end of line.  Many text editors automatically support an editorconfig file when you are working in a repo that has one. Text editors like Visual Studio Code can do the same with a plugin. [More info at EditorConfig.org](https://editorconfig.org/).

It's critical that a label is submitted with the correct end of line character so that Installomator.sh builds correctly. Otherwise a label will start at the end of the previous label's last line, not on a new line. Like in this example: the new label `jamfprintermanager` is inserted but the existing label `jamfreenroller` now starts at the end of the new label's last line, on line 4828.

![end_of_file](https://github.com/user-attachments/assets/a0efcffa-0b38-4fb6-8193-b5ba74651d34)

Check out the tutorials here in the wiki to learn how to build different types of labels.

See [GitHub howto create PRs](GitHub-howto-create-PRs) for how to create Pull Requests in GitHub.
