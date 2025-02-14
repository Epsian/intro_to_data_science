---
pre: <b>9/19. </b>
title: "git"
weight: 6
summary: "Practise some git fundamentals."
format:
    hugo:
      toc: true
      output-file: "_index.en.md"
      reference-links: true
      code-link: true
      
---



-   [Overview][]
-   [Problem Sets][]
    -   [1. Make a New Repository][]
    -   [2. Your First Commit][]
    -   [3. Making a Change][]
    -   [4. Adding More Files][]
    -   [5. Breaking Things][]
    -   [6. Time Travel for Beginners][]
    -   [7. To the Cloud][]
    -   [8. Conflict][]

## Overview

git is remarkably useful, but it takes an investment in using it well to gain the full benefits. Today we're going to try and develop some good habits. While they sometimes feel like a chore to adhere to, if you continue on in data science, good git practice *will* save you some day.

## Problem Sets

### 1. Make a New Repository

All git repositories start with an **initialization**. For us, that will usually mean starting a new R studio project. It is possible to initialize a git repo in a project that you already have files in, and we will cover that process later in the semester. For now, in the upper right corner of R studio, create a project in a new directory called `git_worksheet`, and make sure the "Create git Repository" box is checked when you do.

Once the project is created, find the folder in your file browser or finder window, and double click on the "git_worksheet.Rproj" file to open that R project. In the upper right pane, you should see that there is a tab that says "git."

### 2. Your First Commit

Now that we have a project with a git repository, we can start adding files that we want to keep track of. In general, you want to commit all code files, but not data files. Data files are much larger than what git was made for, and you will quickly run out of storage space on sites like Github if you **push** them. *It is also critically important that you do not commit and files with sensitive information like passwords*. Other people will be able to see them, as git is in no way encrypted. It is also very difficult to remove a file from git's memory; it was made to save things!

<div>

> **Note**
>
> Never commit sensitive files like passwords of API keys.

</div>

Create a new R script file in your project directory called `data_load`. Inside this script, in the fist line write a comment (using `#`) that says "This script downloads the data". Next copy the following code into the script.

    survey = read.csv("https://raw.githubusercontent.com/Intro-to-Data-Science-Template/intro_to_data_science_reader/main/content/class_worksheets/4_r_rstudio/data/survey_data.csv")


    write.csv(survey, "./survey_data.csv", row.names = FALSE)

This code will read our class survey data, and the **write** or save the data in our project directory. Execute both the read and write functions, then save that file once you are done and close it.

Look at the git panel in the upper right pane of R Studio. You should now see (at least) two files there, our `survey_code.R` script, and the `survey_data.csv` file we just saved. Both should have yellow question marks next to them. That means git is not currently keeping track of those files.

We want to tell git to start tracking our `survey_code.R` script. To do so, click on the white check box under the "Staged" column next to `survey_code.R`. Once you click that, the file is **staged**, meaning when we make our next **commit** to the git timeline, that file will be saved.

Let's make out first commit. In the git pane, click on the "Commit" button. This will open the commit window. In this window we will see all our files again on the top left, as well as two new areas. In the top right is a box labeled "Commit message." This is where you can write a message that will appear on the git timeline describing what you are adding or changing in this commit. For now, type in "adding the survey_code script." Press the "Commit" button once you are done. A progress window will pop up letting you know what is happening. Once it stops changing, you can close it and the commit window.

### 3. Making a Change

We've now saved a checkpoint of our `survey_code.R` script, that we can return to at any point in the future. We can even delete it and bring it back from oblivion! For now, we'll just make some changes to it.

Let's add some lines of analysis to the script. open it up, and between our `read` and `write` scripts, add in the following and save your file:

    # Check the varaibles
    str(survey)

    # get the mean hours of sleep
    mean(survey$hours_sleep)

<div>

> **Warning**
>
> git can only ever see changes to your file *after you have saved the file*.

</div>

Now that we've saved out file, it should appear in our git panel again with a blue "M" next to it, signifying the file has been "Modified." Repeat the process of staging it (clicking check box), and committing it (going into the commit menu, adding a message, and pressing commit). We have now added another checkpoint to out git timeline.

### 4. Adding More Files

Thus far, working with one file seems a lot like saving with extra steps. The true value of git starts to appear once we have multiple files in a project. Create a new R script file called `octocat_load.R`, and fill it with the following:

    # Code to load in Octocat (github mascot) art

    octocat = readLines("https://raw.githubusercontent.com/Intro-to-Data-Science-Template/intro_to_data_science_reader/main/content/class_worksheets/6_git/octocat.txt")

    writeLines(octocat, "./octocat.txt")

`readLines` works like `read.csv` in that it is a function to load data into R. Instead of loading CSVs though, it loads text files. Execute this code and then commit the `octocat_load.R`, but *not* the newly created `octocat.txt` file.

Create another new R script called `octocat_print.R`. Inside this file, copy the following code into it:

    # to load the local octocat data and print it

    octocat = readLines("./octocat.txt")

    print(octocat)

Save this file and commit it.

Create one last script called `octocat_count.R`. In this script, copy the following:

    # to count the lines in octocat.txt

    octocat = readLines("./octocat.txt")

    length(octocat)

Save this file and commit it. We now have a toy example of a fairly common data science workflow; get the data, inspect the data, and perform analyses on the data.

### 5. Breaking Things

Now that we have out mini data science workflow, we can start to modify it. Start by opening `octocat_load.R`. We can replace the `readLines` function to load the local version of octocat, because we no longer need to grab it from the internet. Replace:

    octocat = readLines("https://raw.githubusercontent.com/Intro-to-Data-Science-Template/intro_to_data_science_reader/main/content/class_worksheets/6_git/octocat.txt")

With

    octocat = readLines("./octocat.txt")

Save the file and commit the changes.

Say we want to quickly modify our octocat art by adding an extra line for a caption. Create a new script called `octocat_modify.R` and add the following code to it:

    # to add a caption to octocat

    octocat = readLines("./octocat.txt")

    octocat = c(octocat, "ASCII Art of the Octocat Mascot for Github")

    writeChar(octocat, "./octocat.txt")

We use `c()` here to combine octocat with our caption, and then assign it back to our `octocat` object. Save the file, execute it, and commit it.

Great, now we have out data updated, let's open up our `octocat_print.R` and run it again to see our beautiful art again.

Uh-oh. It doesn't work anymore. You may have noticed we used the wrong function to save out modified `octocat` object (we used `writeChar` rather than `writeLines`). That's fine, we can load the data in again in our `octocat_load.R` script... but we can't because we changed that script to use the local copy which we just broke.

Time to power up the time machine.

### 6. Time Travel for Beginners

In the git pane, click on the "History" button to open up the git timeline. The history window is broken in to to main parts. At the top you have your git timeline, which shows all of your commits in this project. The timeline shows you commit messages, the author of those commits, when the commit happened, and an "SHA" which you can think of as a unique ID for that commit. On the bottom is the **diff** or "difference" window. It will show you what files were changed in that commit, and *how* they changes. Sections in green were added, while sections in red were removed.

Find the commit in the timeline where we changed `octocat_load.R`. Inside the **diff** window, on the box labeled `octocat_load.R`, click on the "View file @ \########" button in the upper right of the box. This will open the file as it was when you committed it. Use this to fix our `octocat_load.R` script, and save an working copy of `octocat.txt` again.

### 7. To the Cloud

Now we're going to go over how to set our new repo up on Github. Head to <https://github.com/> and in the left hand side bar, click on the green "New" button. This will take us to the screen to create a new repo. Enter a name under "Repository name" and then scroll to the bottom of the page and click "Create repository."

You will now see a page called "Quick Setup." Look at the second box that says "...or push an existing repository from the command line." We are going to use these commands to link our local repo with the one on Github. In R Studio, look at the lower left console pane, and click on the "Terminal" tab. Enter the three lines of code from Github into the terminal one by one. They should look like this (**but use the ones from Github, not these!**):

    git remote add origin git@github.com:<REPO-DETAILS>
    git branch -M main
    git push -u origin main

Once you have done that, right click or command click anywhere on R Studio and select "Reload." The screen will go blank a moment and return. When it does, go look at the git pane in the upper right. You will notice you now have the option to click on the "Pull" and "Push" buttons. Click "Push" now, wait for the process to finish, then refresh the page for your new repo on github. You should see your files there!

### 8. Conflict

So far so good, but sometimes things go awry. On Github, click on the `octocat_count.R` script to be taken to its page. In the toolbar above the code, on the right hand side, you will see a pencil icon. Click that to edit the file. Add a new comment to the second line that says:

    # Conflict!

Scroll to the bottom of the page and click "Commit Changes." Now, in R Studio, open up `octocat_count.R` and on line 2 there, add a comment that says:

    # It happens.

Save the file and commit it using the git pane.

Now, press the "Pull" button in the git pane. This time an error will pop up saying you have a conflict. A conflict happens whenever git can't reconcile the differences between two versions of the same file, so it will ask you to resolve the conflict. In R Studio, around line 2 in `octocat_count.R`, you should now see the following:

    <<<<<<< HEAD
    # it happens
    =======
    # Conflict!
    >>>>>>> <NUMBERS AND LETTERS>

This is git pointing out where the two versions of the file are different. All the differences will exist between the two rows of arrows, the `<<<<<<<` and `>>>>>>>`. The line of equal signs, `=======`, separates the two versions.

To resolve this, we need to pick which version we want to keep. For now, edit this area, so only the comments that says `# Conflict!` remains. That means you should also delete the `<<<<<<< HEAD`, equal signs, and `>>>>>>> <NUMBERS AND LETTERS>`. Save the file and commit it again. After you commit the changes, press the "Push" button to update github.

  [Overview]: #overview
  [Problem Sets]: #problem-sets
  [1. Make a New Repository]: #make-a-new-repository
  [2. Your First Commit]: #your-first-commit
  [3. Making a Change]: #making-a-change
  [4. Adding More Files]: #adding-more-files
  [5. Breaking Things]: #breaking-things
  [6. Time Travel for Beginners]: #time-travel-for-beginners
  [7. To the Cloud]: #to-the-cloud
  [8. Conflict]: #conflict
