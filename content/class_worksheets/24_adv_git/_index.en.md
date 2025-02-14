---
pre: <b>11/2. </b>
title: "Advanced git"
weight: 24
summary: "git, but more."
format:
    hugo:
      toc: true
      output-file: "_index.en.md"
      reference-links: true
      code-link: true
      
---



-   [Overview][]
-   [git Command Reference][]
-   [Create a new Branch][]
-   [Understanding the Project][]
-   [Feature Creation][]
-   [Make a Pull Request][]
-   [Looking at Other Requests][]

## Overview

git can be a handful, but it is still an invaluable tool. Today we are going to practice using it in a way more like how you would use it in a collaborative environment. This means taking advantage of the branching features in git, along with Github pull requests. The ultimate goal for today will be that you create a pull request with a new feature in the `adv_git` project cloned in class.

If you were not in class, clone the [adv_git project repository][] to your computer.

## git Command Reference

Here I'll provide some of the common git commands for reference.

git status
:   Check the staging area and what branch you are on.

git add \<FILES\>
:   Add a file to the staging area

git commit -m "COMMIT MESSAGE"
:   To save a checkpoint of your project for future reference

git revert \<FILES\>
:   Undo all changes made to a file since the last commit

git push
:   To 'push' files from your local machine onto an internet service

git pull
:   To 'pull' files from a remote service onto your machine

git merge \<BRANCH NAME\>
:   To combine two branches branches. It will merge the branch you name into your current branch.

git checkout \<BRANCH NAME\>
:   Load in a previous checkpoint (commit) or switch to a branch. Use the "-B" flag to create this branch at the same time.

## Create a new Branch

Once you have opened the project, open a terminal and navigate to the project folder. You can also use the "Terminal" tab in the lower left pane of R studio. I need to stress though that *this is just a regular terminal*. It is not a R thing at all. You should not become wed to interface R studio provides as you will not always have it.

Once you have arrived in your project directory, create a new branch in git called `<YOUR NAME>_feature` using `git checkout -B <BRANCH NAME>`. Recall that this command is switching to a new branch that it is also creating because of the `-B` flag (case matters!). You should now be able to type in `git status` and see you are on this new branch in the terminal.

Once you have confirmed this, you will need to link your new branch with Github. Execute the following command, replacing the branch name as appropriate:

    git push --set-upstream origin <BRANCH NAME>

This special push will create a new branch on the remote server ("origin", in our case Github) and link it with your local branch.

## Understanding the Project

The `adv_git` project is set up like a real project would be. The directory structure and how you will contribute to the project reflect how I have actually worked while in government and with collaborators on research. Take some time to understand it. Open each code file, and trace how the various data files are used as inputs and outputs.

All code goes inside the `src` directory, while all data goes in `data`. The code acts on the data in stages, but never alters the raw file. Instead, it first cleans it, the processes it while adding new measures, and all visualization code eventually acts on that processed data file.

All code files also are clearly sectioned. Open up `src/feature_create.R` and look at the outline window on the right. If it is hidden, click on the "Outline" button with three horizontal lines. You'll see each section is clearly labeled, with sub sections for more specific tasks. Try to maintain this structure.

![][1]

## Feature Creation

Now that you are on your own branch, you can push and pull all you want with no fear of merge conflicts! Even if everyone is working on the same file! That's what we'll be doing.

Your task for today is to alter the `src/feature_create.R` file in this project and add some new column to the dataframe. It does not really matter what the feature is. Once you have, you will push your changes to github.

## Make a Pull Request

Once you have pushed your branch, head to Github and look at the [project page][adv_git project repository]. You will see above the file area is a alert that notices you just pushed a branch! It will ask if you want to compare your changes and create a pull request with that branch. Click on the provided green button.

![][2]

This will take you to the pull request page, where you can detail all the changes you made. A pull request is a formal way to request your changes be added to the "main" branch of a project, where the changes will impact other people. It is called a request because this page lets people review your work and make sure your changes won't break anything. On most projects, you can't actually merge your code with the main branch yourself, another team member *has to approve it*. This is to make sure no mistakes you missed get in the "stable" code.

Provide a title for your changes, then give a more detailed description in the provided box. Once you have done that, press the button to create a pull request! You can use the tools on the right to better contextualize your changes by adding labels, link it with long term goals, and assign reviewers to approve your request.

![][3]

## Looking at Other Requests

Once you have turned in your request, look near the top of the page for the "Pull requests" tab. You'll be able to see your request there, along with other your classmates' requests! At the very least, you will find mine titled "Added ability to plot hours of sleep by pet." Click on that.

On this page, you can see the conversation about the request, and more importantly, see the code I changed. Go look at my commits and "Files Changed." Not too much to see, but the principle of the thing stands. You may notice this is the exact same mechanism we have been using to grade your labs! We use the ability to add comments to code line by line in pull requests to give you feedback. You can add a comment if you like by hovering over a line number with your mouse, and clicking the blue `+` button.

  [Overview]: #overview
  [git Command Reference]: #git-command-reference
  [Create a new Branch]: #create-a-new-branch
  [Understanding the Project]: #understanding-the-project
  [Feature Creation]: #feature-creation
  [Make a Pull Request]: #make-a-pull-request
  [Looking at Other Requests]: #looking-at-other-requests
  [adv_git project repository]: https://github.com/Intro-to-Data-Science-Template/adv_git
  [1]: img/outline.png
  [2]: img/pull_request.png
  [3]: img/merge.png
