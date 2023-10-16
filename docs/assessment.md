< Assessment > Shinji Yamamoto 2023/10/02

1. Project workflows
Bryan (2017)

Q. What problems can setwd() cause in your scripts and how do RStudio projects address them?

A. reproducibility of the script will be violated because setwd() sets a path for the author's unique directory. RStudio projects help the organization of each logical project into a folder on your computer, which supports the outcome to be reproducible for anyone who runs the script.


Q. When you call rm(list=ls()), what is removed from your environment? What’s left over that restarting your R session would remove? What’s the keyboard shortcut for restarting your R session?

A. Calling rm(list=ls()) delete user-created objects from the global workspace. Any packages that have been loaded, any options that have been set to non-default values, and the working directory does not get removed. The keyboard shortcuts for restarting the R session are Ctrl+Shift+F10 (Windows and Linux) or Command+Shift+F10 (Mac OS).



2. Version control

You either read Bryan (2018) or Braga et al. (2023). Answer the questions for the paper you read.


Bryan (2018)

Q. The basic git commands are commit, push, and pull. Which commands change happen locally (i.e., on your computer)? Which happen remotely?

A. the command commit happens locally, and push and pull both happen remotely.


Q. Why do diffs work for source code (e.g., .R files) but not Word documents (i.e., .docx files)?

A. the command diffs work ideally with small-to-medium plain text files with hard line breaks, and does not work on binary files like Word documents.


Q. Why is Markdown useful for GitHub repos?

A. With one markdown file GitHub can create a simple website automatically, and it is ideal because it is simple plain text with some markup, but will be displayed like HTML on GitHub.


Braga et al. (2023)

Q. Imagine you’re working with a few collaborators on an analysis. Come up with two examples of Issues you might open. How would using Issues differ from communicating over email?

Q. What are three ways GitHub features can promote open science practices?

---