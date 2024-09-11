---
layout: blog-post
title: Basic Git for versioning scientific analyses

mathjax: false

tags: [tools, tutorials]
---
Git is a powerful tool for keeping track of scientific analyses. So powerful, in fact, that it can be hard to know how to get started. Today we'll try to cover just what you need to get started.

*This tutorial was given at the Ke Lab meeting on Sep. 10, 2024 but should be considered a work in progress!*

## Getting started: command line basics
The most basic way to use Git is from the command line or shell prompt. As this is the most universal way to use Git, we'll be using it for our tutorial today. To get started, open the Command Prompt in Windows (also known as `cmd`) or the Terminal in macOS. You'll see a screen with text; in Windows this will look something like:

```
C:\Users\Joe>
```

and in macOS you might see:

```
joe@Joes-MacBook-Pro ~ %
```

This is called the "command line" or "shell prompt". In both cases, the program is waiting to read your *command*, evaluate it, print the output, and repeat until you're done with your session: this is called the read–eval–print loop (REPL). You will type your commands into the prompt and press enter to run them.

At any time, the shell will be in a *working directory*, which is where it will look for the files you are working with. The most basic commands, and the only ones we will need today for our `git` tutorial, help you navigate your files and change this working directory. The working directory is shown before every command you type. This is `C:\Users\Joe` in our Windows example, and `~` in our macOS example, which is an abbreviation for your *home directory*, which is something like `/Users/joe`.

### Basic structure of commands

Before we get into the specific commands, a note on how to use commands: a command can be made up of multiple parts, separated with a space. For instance, to navigate to the folder named `example_analysis`, we would run the command (in either Windows or macOS/UNIX):

```
cd example_analysis
```

Here, the space separates the command (`cd`, which tells the shell we want to change to a different folder) from its argument (`example_analysis`, which tells the command which folder we want). This means that we have to treat the argument specially when there is a space in it: we surround it with quotation marks `"`: supposing our folder is called `Example Analysis`, we would then need the command:

```
cd "Example Analysis"
```

> **Quick tip:** If you need help with a command, you can usually ask it for help by giving a special argument. On Windows, use `/?`  after the command you want to know more about. Typing `cd /?` you might see the following:
>
> ```
> C:\Users\Joe\Desktop\Git Tutorial>cd /?
> Displays the name of or changes the current directory.
> 
> CHDIR [/D] [drive:][path]
> CHDIR [..]
> CD [/D] [drive:][path]
> CD [..]
> 
> ...
> ```
>
> On macOS/UNIX, use `--help`. So, with `cd --help` you will see the following:
>
> ```
> joe@Joes-MacBook-Pro Git Tutorial % cd --help
> usage: cd [-L|-P] [dir]
> Change the current directory to DIR. The default DIR is the value of the HOME environment variable.
>
> ...
> ```
>
> Note that some tools on Windows (mostly those that also run on macOS/UNIX) also use `--help` instead of `/?`. This includes Git itself. If in doubt, try it out and see. Later we will see that arguments starting with `--` are common ways of selecting options for commands; they can often also be abbreviated to `-`: for instance `-h` is a common abbreviation for `--help`.

### Navigating the file system

With this in mind, let's navigate straight to the example directory we're using today. Make a folder somewhere convenient on your computer for today's tutorial; I've made them on my desktop.

Every folder or file on your system has its own *path* identifying where exactly it is. We need the path we'll navigate to. In Windows this might be `C:\Users\Joe\Desktop\Git Tutorial\`, where `\` separates the parts (folders or filename) in the path; in macOS we might have `/Users/joe/Desktop/Git Tutorial/` (note that macOS/UNIX use a different character `/` here!). Because we've chosen an example with spaces, we need to put quotation marks around the path; on Windows, the command is:

```
cd "C:\Users\Joe\Desktop\Git Tutorial\"
```

and on macOS:

```
cd "/Users/joe/Desktop/Git Tutorial/"
```

(or, for macOS/UNIX, `"~/Desktop/Git Tutorial"`, remembering that `~` is a shortcut for your home directory).

> **Quick tip:** On Windows and macOS, you can drag and drop the file into the terminal window or copy-paste it instead of typing the path by hand.

Now we can see what files are in our working directory. You can do this simply by typing the `dir` (Windows) or `ls -a`  (macOS/UNIX) command. On Windows you will see:

```
C:\Users\Joe\Desktop\Git Tutorial>dir
 Volume in drive C has no label.
 Volume Serial Number is XXXX-XXXX

 Directory of C:\Users\Joe\Desktop\Git Tutorial

09/09/2024  12:00 PM    <DIR>          .
09/09/2024  12:00 PM    <DIR>          ..
               0 File(s)              0 bytes
               2 Dir(s)  123,456,789,012 bytes free
```

and on macOS:

```
joe@Joes-MacBook-Pro Git Tutorial % ls -a
.   ..
```

Our folder is empty, so why do we have the items `.` and `..` in our output? These represent the current folder (`.`) and the folder above (parent) of the current one. So, we could use `cd ..` (Windows or macOS/UNIX) to go back to that folder (`C:\Users\Joe\Desktop\` or `/Users/joe/Desktop/`).

> **Exercise:** Use `cd ..` to go back to the previous directory. Then use `dir`/`ls` to view its contents. Finally, use a command beginning with `cd`  to go back to our `Git Tutorial` folder.

> **Quick tip:** Press the Tab key and the terminal will try to complete a command for you. Repeat the exercise, but press Tab after typing "Git" and it will complete the name of the folder for you (assuming you don't have any other folders starting with those letters).

### Command line reference table

Here are some of the most commonly used commands. We'll only need the first four today, but you might use the others later.

| Command                                                | Windows                                                      | macOS/UNIX                                                   |
| ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Show current path                                      | `cd` or `chdir`                                              | `pwd`                                                        |
| List files and folders                                 | `dir`                                                        | `ls -a` (for **a**ll files) or `ls` (non-hidden files only)  |
| Change to a different path                             | `cd [PATH]` or `chdir [PATH]`                                | `cd [PATH]`                                                  |
| Go back to the parent folder of the current path       | `cd ..` or `chdir ..`                                        | `cd ..`                                                      |
| Clear old output from terminal                         | `cls`                                                        | `clear`                                                      |
| Stop the currently running process (keyboard shortcut) | Ctrl + C                                                     | Command + . (macOS) or Ctrl + C (Linux)                      |
| Get help with command                                  | `[COMMAND] /?` (sometimes `[COMMAND] --help` or `[COMMAND] -h`  for programs shared with macOS/UNIX) | `[COMMAND] --help` or `[COMMAND] -h` (or `man [COMMAND]` for even more information; press the "q" key to quit). |
| Move or rename file                                    | `move [PATH1] [PATH2]`                                       | `mv [PATH1] [PATH2]`                                         |
| Copy file                                              | `copy [NAME1] [NAME2]`                                       | `cp [NAME1] [NAME2]`                                         |
| Make folder                                            | `md [FOLDER]`                                                | `mkdir [FOLDER]`                                             |
| Delete file **(permanent!)**                           | `del [FILE]`                                                 | `rm [FILE]`                                                  |
| Delete folder                                          | `rmdir [FOLDER]`                                             | `rmdir [FOLDER]`                                             |

> **Quick tip:** If you get stuck, you can return to the prompt by pressing the key combination Ctrl + C (Windows, Linux) or Command + . (macOS). Please be careful though, as this will completely stop whatever you're currently running!

## Installing and setting up Git (and Github CLI)

Now that we know how to use the command line, let's finally get around to installing the Git tool. We'll also install the command line interface from Github to help us connect to our account; I've gotten this part of today's info from their [very helpful guide](https://docs.github.com/en/get-started/getting-started-with-git) to getting started.

#### Installing from the command line

To install Git, use the *package manager* for your system. This is something like the command line equivalent of the App Store; it will download and install (and even update or uninstall) software for you. On Windows we will use `winget`. Type:

```
winget install -e --id Git.Git
```

and go through the installation. On macOS we will use Homebrew; we need the following command:

```
brew install git
```

After everything is done, close the command line terminal. Then open it again and try running the `git`  command. If your installation worked, you should see something like the following:

```
C:\Users\Joe\Desktop\Git Tutorial>git
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone             Clone a repository into a new directory
   init              Create an empty Git repository or reinitialize an existing one
...
```

#### Install Github CLI

Now do the same with Github CLI. In  Windows, the command is:

```
winget install -e --id GitHub.cli
```

and on macOS:

```
brew install gh
```

As before, follow instructions to install, restart your terminal, and run the `gh` command to see if it worked.

#### Set up your installation

When you make updates with Git, it tags everything you do with your name and email. We need to tell Git which name and email to do. In Windows or macOS, use the following two commands to tell it this information:

```
git config --global user.name "Joe Wan"
git config --global user.email "joewan@example.com"
```

You can use any name (this is not the same your GitHub username), but most people use their full name. The email should match what you used for you GitHub account. You can change these settings whenever you want by running these commands again.

Check that things worked by running `git config --global user.name` and `git config --global user.email`. You should see the following:

```
C:\Users\Joe\Desktop\Git Tutorial>git config --global user.name
Joe Wan
C:\Users\Joe\Desktop\Git Tutorial>git config --global user.email
joewan@example.com

```

Now we need to link the command line `git`  tool with your account on the GitHub server. To do so, run the following command:

```
gh auth login
```

Follow the instructions and log in when the browser window comes up. If everything worked, you should see something like the following:

```
C:\Users\Joe\Desktop\Git Tutorial>gh auth login
? What account do you want to log into? ...
- Logging into ...
? How would you like to authenticate GitHub CLI? Login with a web browser
- Press Enter to open GitHub in your browser...
✓ Logged in as Joe Wan
```

Now Git will securely store your login so that you can interact with your data stored on GitHub's servers.


##  First steps: create repository, stage, and commit

Let's get started; today we'll pretend we are developing an analysis in R. Create a file in your `Git Tutorial` directory called `analysis.R` with the following code:

```R
#!/usr/bin/env Rscript
# analysis.R
# ==========
# Runs the example analysis.

library(ggplot2)

# Some example data
data <- data.frame(id=c(1, 2, 3, 4, 5), species=c('M', 'M', 'M', 'E', 'M'),
   competitor.num=c(0, 1, 2, 0, 4), biomass=c(1.05, 0.52, 0.61, 2.15, 0.18))

# Plot biomass vs. number of competitors
ggplot(data) +
   geom_point(aes(x=competitor.num, y=biomass))
```

> **Review:** Use the command line to check if the file you created is there in your working directory.

### Creating a repository with `init`
Now we're ready to start using Git to version control this research project. Creating a new git repository is simple: use the `git init` command. Successful output looks like this:

```
C:\Users\Joe\Desktop\Git Tutorial>git init
Initialized empty Git repository in C:/Users/Joe Wan/Desktop/Git Tutorial/.git/
```

A repository holds your files and their history for one project. Git stores your local copy of this repository in the special folder `.git`, so make sure not to delete or modify it!

> **Exercise:** List the contents of your working directory (check the table above if you've forgotten the command) and verify that there is now a `.git` directory. For bonus points, navigate into the `.git` directory and see what's inside. When you're done, make sure to go back to your `Git Tutorial` directory! 

### Checking the `status`
At any time, we can check what's happening with our current repository using the command `git status`. The output will look like this:

```
C:\Users\Joe\Desktop\Git Tutorial>git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	analysis.R

nothing added to commit but untracked files present (use "git add" to track)
```

This tells us that git has noticed a new change in our project. 

### Staging with `add`
Helpfully, the output also tells us how we can add it to our project. Let's follow the instructions and add our script file using `git add analysis.R`, then check the status again:

> **Quick tip:** instead of typing out the entire name, remember you can use the Tab key to have the terminal complete it for you!

```
C:\Users\Joe\Desktop\Git Tutorial>git add analysis.R
C:\Users\Joe\Desktop\Git Tutorial>git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   analysis.R
```

Now `analysis.R` is *staged*. This means that Git knows about our changes to this file (i.e. that we've created it) and is ready to record them.

### Storing changes with `commit`
However, be careful: the changes are not yet saved! To do so we need to *commit* our staged changes, telling Git to save a record of them. We can do this with the `commit` command. In Git, each commit, representing one step in the development of the project, had a *commit message* summarizing what was changed. We provide this to the commit command using the `-m` flag, so the entire command might be `git commit -m "Your commit message here..."` (**Review:** why are there quotation marks around the commit message?). Typically, a commit message is short (up to 50 characters) and describes the reason why you made a change, in present tense (e.g. "Add new analysis for biomass"). Since this commit is our first in the repository, we might choose this following message and see the corresponding output:

```
C:\Users\Joe\Desktop\Git Tutorial>git commit -m "Initial commit with analysis script"
[main (root-commit) b86d026] Initial commit with analysis script
 1 file changed, 14 insertions(+)
 create mode 100644 analysis.R
```

> **Quick tip:** If you leave out the `-m "Your commit message here..."` in the command, Git will take you to an editor screen where you must write out the command. Follow instructions to save and exit after you've written the message. (This can be tricky if you're not familiar with the editor, so I prefer to just provide the message in the command).

If we run `git status` again, Git will tell us that all our changes have successfully been committed to the repository.

```
C:\Users\Joe\Desktop\Git Tutorial>git status
On branch main
nothing to commit, working tree clean
```

Great! Let's practice what we've learned with a little exercise:

> **Exercise:** Create a text file named `README.md` with the following text:
>
> ```
> # Example analysis
> This Git repository contains an example analysis for our Git tutorial. Files:
> - `analysis.R`: The R script
> ```
> 
> This is called the README file and always has the name `README.md` or `README.txt`, and is the standard way to summarize a project (or part of a project). Now, stage and commit it to your repository using the same commands from above. Make sure to write an informative but short commit message!


## The power of Git: tracking and working with history

Now, in addition to our actual project files (`analysis.R` and `README.md`), we have a record of the changes we made throughout the project stored in our Git repository. Why is this better than simply having a backup? We'll see why Git is so powerful as we learn how to access and use this stored history. 

### Checking the `log`
Run the command `git log` to see an overview of the history so far:

```
C:\Users\Joe\Desktop\Git Tutorial>git log
commit 495b1d9668b9ef7e1e47434e1f4ffcddce64c743 (HEAD -> main)
Author: Joe Wan <joewan94@gmail.com>
Date:   Tue Sep 10 15:21:15 2024 +0800

    Add README file with description of repository

commit b86d0264e52dcb1b2033d83a3e540e192505bb6e
Author: Joe Wan <joewan94@gmail.com>
Date:   Tue Sep 10 15:01:39 2024 +0800

    Initial commit with analysis script
```

This command lists all the changes Git remembers, with the newest at the top (the text `(HEAD -> main)` indicates that that commit is the current saved state of the project). The long string of digits and numbers `495b1d9668b9ef7e1e47434e1f4ffcddce64c743` is called a *commit hash* and is a unique ID which we will use to refer to the commit later.

### Viewing changes using `diff`
The real advantage to this comes when we have made many changes to our project file over time. Let's modify our script file and see what happens. Add the following lines to the script, before the line `# Plot biomass vs. number of competitors`.

```
# Only use individuals of species M for the analysis
data <- data[data$species == 'M', ]
```

Running `git status` again shows that Git knows we've changed the analysis. This time, we can also run the command `git diff` to show what exactly has changed.

```
C:\Users\Joe\Desktop\Git Tutorial>git diff
diff --git a/analysis.R b/analysis.R
index a0da279..59c49fd 100644
--- a/analysis.R
+++ b/analysis.R
@@ -9,6 +9,9 @@ library(ggplot2)
 data <- data.frame(id=c(1, 2, 3, 4, 5), species=c('M', 'M', 'M', 'E', 'M'),
    competitor.num=c(0, 1, 2, 0, 4), biomass=c(1.05, 0.52, 0.61, 2.15, 0.18))
 
+# Only use individuals of species M for the analysis
+data <- data[data$species == 'M', ]
+
 # Plot biomass vs. number of competitors
 ggplot(data) +
    geom_point(aes(x=competitor.num, y=biomass))
```

The lines starting with `+` show us what's new in this file; you should see that the only difference is the new lines you added. Now, go ahead and use the `add` and `commit` commands to add our changes to the repository, then use the `log` and `status` commands to check that your changes have successfully been committed.

> **Exercise:** The `diff` command works with old commits as well as new changes. The syntax is: `git diff [VERSION1] [VERSION2]` to see what changed between two versions, where `[VERSION1]` and `[VERSION2]` are the commit hashes (which, you'll remember, we found in the output of `git log`). Look of the commit hashes of the first and last commits, and use `diff` to view all changes we've made in our project.

> **Quick tip:** Save some typing! You only need to type the first four characters of each commit hash, and Git will figure out which one you want. You can also refer to the last commit with `HEAD` rather than its commit hash, and add `~1` (or some other number `~N`) to go back 1 (or `N`) commits: so `git diff HEAD~1 HEAD` shows the changes between the last two commits.

### Discarding current changes using `restore`
Git also allows you to quickly restore old versions of your repository. Say we've somehow broken our analysis script: go ahead and delete all the text from `analysis.R`. Use `status` and `diff` to check what's happened. Git still remembers the last working version of the analysis! We can restore that version (discarding our bad changes) by using the `restore` command, which restores the version in the repository. Tell it to restore your committed version of `analysis.R`:

```
C:\Users\Joe\Desktop\Git Tutorial>git restore analysis.R
C:\Users\Joe\Desktop\Git Tutorial>git status
On branch main
nothing to commit, working tree clean
```

Open the analysis script again and you'll see that we've successfully recovered the good version of our script!

> **Quick tip:** If you've already staged a change that you don't want, you can use `restore` with the `--staged` to remove it from the list of staged changes: `git restore --staged [FILE]`. Note that unlike plain `git restore`, the changes you've made will still be in the file.

### Restoring old versions with `checkout`
This principle also applies to old versions of our analysis. Say we've decided not to go with the new analysis direction from when we committed the two new lines to `analysis.R`. We can restore the version of the file from the last commit using the `checkout` command. The syntax is `git checkout [VERSION] -- [FILE]` (note the `--` and the spaces before and after it!), which restores just `[FILE]` from the commit `[VERSION]` (which can be a commit hash or abbreviation, as above). So we'll run the following to restore `analysis.R` to its state before we added the lines:

```
C:\Users\Joe\Desktop\Git Tutorial>git checkout HEAD~1 -- analysis.R
```

> (**Review:** what does `HEAD~1` do in this version? Which commit hash could we have typed in its place?)

This stages the version of the file we asked for, which should have removed the new lines. To confirm, we can use a special option for the `diff` command, `--staged`, which shows the difference between the staged changes and the last commit. Running `diff` with the `--staged` flag, we see the following output:

```
C:\Users\Joe\Desktop\Git Tutorial>git diff --staged
diff --git a/analysis.R b/analysis.R
index 59c49fd..a0da279 100644
--- a/analysis.R
+++ b/analysis.R
@@ -9,9 +9,6 @@ library(ggplot2)
 data <- data.frame(id=c(1, 2, 3, 4, 5), species=c('M', 'M', 'M', 'E', 'M'),
    competitor.num=c(0, 1, 2, 0, 4), biomass=c(1.05, 0.52, 0.61, 2.15, 0.18))
 
-# Only use individuals of species M for the analysis
-data <- data[data$species == 'M', ]
-
 # Plot biomass vs. number of competitors
 ggplot(data) +
    geom_point(aes(x=competitor.num, y=biomass))
```

This looks good, so go ahead and commit your staged changes with an appropriate message. Our entire history looks like this:

```
C:\Users\Joe\Desktop\Git Tutorial>git log
commit a8e05c89d5c0d144387ea5de36231ca3d24a4497 (HEAD -> main)
Author: Joe Wan <joewan94@gmail.com>
Date:   Tue Sep 10 16:36:04 2024 +0800

    Revert analysis to old version with all species

commit cbf0c3d94cc084ff51a9ad682d1c82b9eaebe33e
Author: Joe Wan <joewan94@gmail.com>
Date:   Tue Sep 10 15:41:12 2024 +0800

    Restrict analysis to species M only

commit 495b1d9668b9ef7e1e47434e1f4ffcddce64c743
Author: Joe Wan <joewan94@gmail.com>
Date:   Tue Sep 10 15:21:15 2024 +0800

    Add README file with description of repository

commit b86d0264e52dcb1b2033d83a3e540e192505bb6e
Author: Joe Wan <joewan94@gmail.com>
Date:   Tue Sep 10 15:01:39 2024 +0800

    Initial commit with analysis script
```

Even though we've gone back to our older script version, Git remembers everything we've ever committed to it! This is the great advantage of using content control: we can feel free to experiment with our code, knowing that (as long as we commit regularly) we can always go back to a working version of our analysis.

> **Exercise:** Oops, we've decided that we only want to analyze species M after all! Check the log to identify the version we need (which should be easy because we've written such clear and helpful commit messages) and repeat the steps with `checkout`, etc. to restore the analysis we need. You can use `HEAD~N` or the commit hash, as you want, but use `diff` to check the staged changes to make sure you're getting what you intended.


## Getting online: working with remotes

So far, everything we've done is on our own computer. This is useful if you're coding without internet connection, but if you want your repository to be safely backed up or share it with others (or work on this project your other computer) you will need to connect your repository to an online server.

This is where *remotes* come in. A remote is a version of your repository stored on a server; by sending any changes to your repository to the server, you have a safely stored version of all the history that you or others can access on different computers. There are many services that provide this (and you can even host your own remote), but the most popular one is [GitHub](https://github.com).

### Connecting your local repository to GitHub
Log into GitHub. On the left side of your home page, you will see a (currently empty) list of repositories; click the green "New" button above this list to create a new repository. Give your repository a descriptive name (I'm using `git-tutorial` today), a short (optional) description, and select the private. Keep the other options at their default (don't add README, .gitignore, or license files).

> **Quick tip:** Keep your repository names concise, but make sure they're specific; `psf-project` may be fine for now, but will you remember what it is after doing ten more plant soil feedback projects? You should also try to be consistent with your naming format: the most common way to format repository names is in lowercase with words separated by dashes (`my-repository-name`) but other ways are also fine (`my_repository_name`, `MyRepositoryName`, etc.); just pick one and stick with it!

If this worked, you will be taken to a page that tells you how to set up your repository. We already have a repository, so we'll use the instructions under "...or push an existing repository from the command line". Copy and paste these three commands into the terminal (yours will have a different URL, so use the ones from GitHub!):

```
C:\Users\Joe\Desktop\Git Tutorial>git remote add origin https://github.com/joe-wan/git-tutorial.git
C:\Users\Joe\Desktop\Git Tutorial>git branch -M main
C:\Users\Joe\Desktop\Git Tutorial>git push -u origin main
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 8 threads
Compressing objects: 100% (10/10), done.
Writing objects: 100% (11/11), 1.46 KiB | 375.00 KiB/s, done.
Total 11 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/joe-wan/git-tutorial.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

As you watch, your repository data has been uploaded to GitHub! 

> **Quick tip:** If you prefer to stay on the command line, you can even use the GitHub CLI tool (`gh`) to create a GitHub repository for your project. From your working directory, simply run `gh repo create` and follow the prompts.

### Explore your online repository
Refresh the GitHub page and you will see that all your project files and commits are now on the GitHub server (but remember, since you selected "Private", you are the only one who can see them). This can be a quite convenient way to browse your files and their history. Click on `analysis.R` and you'll see the code we've just written. Above the code you'll see a bar with your most recent commit to this code; if you click on the click icon on the right of this bar, you can see *all* commits that affected this file and view what they changed, just as you did with the `diff` tool on the command line. Go back to the file and click "Blame" in the top right corner of the code box. This very powerful tool will show you exactly when you added each of the lines in the current script.

> **Exercise:** You can also "blame" on the command line. Run the command `git blame analysis.R` and compare the output to what you see on the online interface.

### Synchronizing with `push` and `pull`
Now there is one more thing to remember as we're developing our analyses. In addition to committing our changes to our own *local* repository (which, you'll remember, lives in the `.git` folder of our own project folder), we'll have to make sure our changes match the ones on the GitHub server. To do so, we'll use the `pull` and `push` commands.

The `pull` command checks GitHub's version of your repository (online) for any new changes, and downloads them into your local repository (in the `.git` folder):

```
C:\Users\Joe\Desktop\Git Tutorial>git pull
Already up to date.
```

If you're the only one (and only computer) working on a project, you shouldn't expect to have any new changes on the server. However, it's good to get into the habit of regularly pulling because you might one day have to collaborate on Git, or want to work on a different computer. If the repository changes while you are working on it, you may need to resolve any resulting conflicts. This is something called `merging`, and can be quite complex, so it's best to try to avoid this situation for now.

> **Review:** Let's make some changes to the script, create some additional files, and push them to the server. Say we want to save the graph produced by the script. Add the following lines to the end of the script:
> 
> ```
> # Save our graph
> ggsave('output.pdf')
> ```
> 
> Now run the script from your working directory to create the graph. Command line fans can actually do this directly from the terminal using `Rscript analysis.R`, which automatically runs from the current working directory, but feel free to do this in RStudio if you prefer (just make sure to set the working directory correctly!).
> 
> You now have some new files. Stage them all and commit to your local repository.

> **Quick tip:** use `git add .` instead of adding each file individually; recall that `.` is the current directory.

After completing these familiar tasks, you're ready for the new step: pushing the changes to the remote (online) repository. This is done simply by typing `git push`:

```
C:\Users\Joe\Desktop\Git Tutorial>git push
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 8 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 5.70 KiB | 1.90 MiB/s, done.
Total 6 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To https://github.com/joe-wan/git-tutorial.git
   d94fbc8..26d8f22  main -> main
```

Now everything is on the server, as you can confirm by visiting the online interface for your repository again (you can even preview the PDFs of your graphs!). You now have all the skills you need to get started versioning your analyses with Git.

### Getting another copy of your project with `clone`

Git is also a great way to distribute copies of your software (many use it to share code for published analyses, combined with [services that permanently archive the repository](https://docs.github.com/en/repositories/archiving-a-github-repository/referencing-and-citing-content)). To see how this works, we'll create a second copy of our repository elsewhere on our computer.

We're not allowed to create a copy of the repository inside itself. In the terminal, navigate outside of your current repository folder (for example, **Review:** change to the parent directory of your working directory; e.g. here, the `Desktop/` folder).

We're ready to get a copy of the repository from the GitHub server. To do so, we'll use Git's `clone` command; the syntax is `git clone [URL]`. Git will look for the data at a special URL called the *remote url*. (Don't worry, because you selected "Private" when creating the repository, only you can clone from this URL!) To find this, go to your repository's page in GitHub and click on the dropdown arrow on the green "<> Code" button. Copy the URL under the tab that says "HTTPS". Run the command and you'll see:

```
C:\Users\Joe\Desktop>git clone https://github.com/joe-wan/git-tutorial.git
Cloning into 'git-tutorial'...
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 22 (delta 4), reused 22 (delta 4), pack-reused 0 (from 0)
Unpacking objects: 100% (22/22), 7.68 KiB | 1.54 MiB/s, done.
```

> **Quick tip:** By default, `clone` will create a new folder with the same name as the repository, here `git-tutorial`. If you want to clone into a folder with a different name, you can add this name after the URL: `git clone [URL] [FOLDER]`.

Now you have a second copy of the repository, with all the files exactly the way they were when you last committed them. This could be useful if you're setting up the same analysis on a second computer. Also, though Git offers a more official way to do this (`stash`), cloning your repository is an easy way to quickly run an old version of your analysis without losing your current progress: after cloning use `git checkout [COMMIT]` (with the commit hash you need) to get the old version, and just delete the extra cloned directory when you're done.

We're done with this folder, so feel free to navigate back to the original folder and delete the clone. Or, if you like, stay here because it doesn't matter—that's the beauty of Git!


## Git hygiene: when and what to commit

Now that you know the basics, let's discuss some tips for getting the most out of Git. These are not hard rules, but are widely practiced conventions that help you write better code and work more efficiently.

### The `pull`–`add`–`commit`–`push` cycle
In general, during a coding session, your workflow might look like this:

1. Pull changes from your other machine or collaborators with `git pull`.
2. Get a new step of your analysis working.
3. Stage (`git add [FILE]`) the changes.
4. Make sure everything works, then commit with a message describing the specific stem you made (`git commmit -m "[MESSAGE]"`).
4. Push your changes to the repository (`git push`).
5. Repeat 1-4 until you're done with your coding session.

We only did a toy analysis today, but even when working on real code, it's a good idea to make incremental changes and test in a small cycle like this. By committing and pushing regularly, you ensure that any changes you make are backed up and recorded. Also, by making sure each commit has a single clear goal, you make it easier to go back to an exact previous version of your analysis. For the same reason, it's also best to avoid pushing code that you know doesn't work. 

> **Advanced tip:** Later in your Git journey, you can learn to use the branch feature to more comfortably make experimental changes that could break your analysis.

### Keeping your repository clean (deleting with `git rm`)
In addition, it is good to be careful about what you commit. Let's go back to our example. List the files in the current version of the repository (everything was automatically staged and committed because I used `git add .`). 

```
C:\Users\Joe\Desktop\Git Tutorial>dir
 Volume in drive C has no label.
 Volume Serial Number is XXXX-XXXX

 Directory of C:\Users\Joe\Desktop\Git Tutorial

09/10/2024  12:00 PM    <DIR>          .
09/10/2024  12:00 PM    <DIR>          ..
09/10/2024  12:00 PM                36 .Rhistory
09/10/2024  12:00 PM               504 analysis.R
09/10/2024  12:00 PM              4822 output.pdf
09/10/2024  12:00 PM               126 README.md
09/10/2024  12:00 PM              4811 Rplots.pdf
               4 File(s)            492 bytes
               2 Dir(s)  123,456,789,012 bytes free
```

In addition to our changes to the script and the new output, there are several other files that automatically generated when I ran the script: .Rhistory (a list of commands typed in R or RStudio) and Rplots.pdf (an extra file with all plots that were shown); depending on how you ran the script, you may have a different set of files. We shouldn't commit these files because we don't need them, they get in the way of seeing actual important changes, and in any case may be different on other computers. This is a good opportunity to learn how to delete files with Git. The general syntax is `git rm [FILE]`. (**Warning:** don't confuse this with plain `rm`, which permanently deletes a file on macOS/UNIX!) 

```
C:\Users\Joe\Desktop\Git Tutorial>git rm .Rhistory Rplots.pdf
rm '.Rhistory'
rm 'Rplots.pdf'
```

This works just like any other change in Git: these files have disappeared (**Review:** confirm this by listing the files in your working directory), but these changes have only been staged and we could restore the files if we wanted:

```
C:\Users\Joe\Desktop\Git Tutorial>git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	deleted:    .Rhistory
	deleted:    Rplots.pdf
```

Commit and push your changes to remove the files. Unlike deleting a file normally, these files will always stay in the history of our repository. This is an advantage because once you commit a change, you never need to worry about permanently losing that information; it can always be restored at a later date.

> **Advanced tip:** In cases when accidentally committing the wrong file creates problems (e.g. a large file or sensitive data), there are [more advanced ways](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository) to retrospectively remove data from the repository. As this is annoying and potentially risky, it's best to avoid this kind of mistake in the first place.

Note that renaming files works similarly to deleting: to rename (move) a file, run the command `git mv [FILE] [NEWPATH]` and the file rename will be staged for you. This also works if you want to move files into other directories. (Interestingly, Git detects file moves automatically, so deleting a file and recreating it under a different name is indistinguishable from `git mv`-ing it.)

### Automatically keeping clean with the `.gitignore` file
In the future we'd like to avoid accidentally committing these files in the first place. The best way to do this is the `.gitignore` file, which tells Git which files to avoid committing. Make a new file called `.gitignore` in the current directory with the following text:

```
# Ignore the history file
.Rhistory

# Ignore the automatically saved plots
Rplots.pdf
```

> **Quick tip:** You can also use `*.[EXTENSION]` to ignore all files of a certain type: `*.pdf` would ignore all PDF files, for example.

> **Quick tip:** Your repository can contain subfolders (in fact, this is the norm), and each subfolder can have its own `.gitignore` file (which must have *exactly* this filename). This makes it easy to have different rules for different parts of your project, or to ignore a single specific file.

If you have other files produced by running the script, you can also list them in this file so they are ignored. Now, stage and commit the `.gitignore` file. Run the script again, which will re-generate the files we just deleted. However, when you run `git status`, you will see that Git now ignores those files, only showing the change in `output.pdf`. The extra files will no longer be staged or committed even if you run `git add .`. (However, note that if you specifically add them, it is still possible to commit ignored files to the repository.)

GitHub has an official collection of `.gitignore` files for different programming languages [here](https://github.com/github/gitignore/tree/main); the ones for [R](https://github.com/github/gitignore/blob/main/R.gitignore) and [LaTeX](https://github.com/github/gitignore/blob/main/TeX.gitignore) may be particularly useful for you.

### General tips: what to commit?
In general, what should we commit to our repository? Git works best with text-based files: this is where we can use the features that really make it powerful (like `diff`). Conversely, as it stores each version of each file that is committed forever, Git will have problems with large non-text files that often change. (GitHub will actually warn you if your file is larger than 50 MiB, and it will refuse to allow you to push files larger than 100 MiB!) With that in mind, here are some recommendations on what to include in your repository:

**Definitely** include:
- **Code files:** the original use case for Git, and where it really shines!
- README files: It's good practice to include a file called `README.md` or `README.txt` that describes what's going on in a repository (or folder inside a repository). This is especially important if you're sharing publicly (e.g. you will likely need to share code for published manuscripts); see [here](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) for tips on how to format using Markdown (`.md`)
- **Written documents in text-based formats:** this includes LaTeX as well as Markdown, RMarkdown, text files (`.txt`) etc. (If you are using LaTeX, make sure to configure your `.gitignore` to avoid annoying intermediate files, which can also cause problems for collaborators.)
- **Text-based research data:** Git is actually a great way to track changes to your data if you use a text-based format like CSV. Spot a typo in a row? You can simply change the `.csv` file and Git will be able to tell you exactly what you corrected at any point in the analysis. If the CSV is automatically generated by a script, however, best practices may vary; see below.
- ***Small non-text files** needed for running your analysis: you should usually include these for convenience; Git can handle these without too much trouble (e.g. data in other formats, images and figures you need for your LaTeX document).

> **Quick tip:** It's good to remember that Git looks at things line-by-line when you use tools like `diff`. In text documents like LaTeX, keep lines relatively short: some people like to put each sentence on its own line. In text and code, avoid automatically formatting everything at once (e.g. fixing indentation using an automatic formatter in your code editor, or automatically making all the lines of text in your LaTeX document 80 characters long), though there are ways to make Git ignore such changes. If you *must* do this, do it in its own separate commit, not together with other changes.

**Maybe** include:
- **Documents in non–text-based formats** (e.g. Word): As these files are usually not too big, Git can handle them, but you won't be able to benefit from version control. Consider storing documents in a more text-based format (see above); you may also find that these formats are faster to use because you don't need to worry as much about layout on the page. There are Word-like graphical editors for these too: for Markdown e.g. [Obsidian](https://obsidian.md/) and [marktext](https://github.com/marktext/marktext); for math I like to use [LyX]([https://www.lyx.org/]) which is TeX-based.
- **Outputs of your scripts:** Traditionally (in the programming world) the advice was not to put things that can be generated from your code under version control. However, this can be quite convenient in scientific contexts if files are small (e.g. storing your graph outputs as `.pdf` or `.png` alongside the scripts). In this case, make sure to keep them updated and always commit them along with changes in the script (otherwise you may accidentally describe the wrong methods for a particular figure version!).
- **Important versions of your documents and analyses:** Many like to store submitted versions of projects in a separate folder inside the same repository for easy access. Similarly, if you're working on two slightly different versions of an analysis in parallel, you can just copy the folder. Git has features that can cover these use case (branches, tags, releases, etc.), but for our needs it may be too troublesome.

**Don't** include:
- **Clutter:** Set up your `.gitignore` for the programming language you're using so you don't clutter your repository with unneeded files.
- **Large input datasets:** This includes e.g. large sequencing datasets such as raw metabarcoding data (GitHub will refuse these in any case). The best practice is to keep them somewhere else and write your code to use the data from that location. For files of up to 2 GiB, you can use [Git LFS](https://git-lfs.github.com/) to automatically store a copy of these files on GitHub's servers and link them to your repository.
- **Large output files:** Git will keep each version of every file. Say you save your simulation results and each run is 100 MiB (which Git allows): after changing the parameters 10 times and committing, you are now storing 1 GiB of old data. Git is not for backing up this kind of data!

> **Advanced tip:** You can avoid losing the information stored in large output files by making sure your code is reproducible (for instance, [set a random seed in R](https://uc-r.github.io/setting_seed/) if your analysis uses random numbers). If you commit the code for every working version of the analysis, you should be able to re-generate the data for any version just by running the code.
>
> If you really want to make sure large output files don't get lost (e.g. if the simulations took a week to run), consider using your lab's storage system or a cloud-based service (which can keep old versions of your files for e.g. a month). I keep these kinds of research outputs in an `outputs/` folder which is ignored using `.gitignore`, and back them up with Dropbox.
>
> Saving your data in an appropriate format and compressing it (e.g. [using RDS](https://readr.tidyverse.org/reference/read_rds.html) with the `compress="gz"` option) may also be useful in any case.


## Review: table of commonly used Git commands

Let's summarize what we've learned in a table:

| Task | Git Command | Notes |
| --- | --- | --- |
| Create a repository | `git init` ||
| Check the status of your repository | `git status` ||
| Stage (add) a changed file | `git add [FILE]` | `git add .` will stage all changes in the working directory|
| Store (commit) the staged changes | `git commit -m "[MESSAGE]"` ||
| View the history of your repository | `git log` ||
| View the new changes in your working directory | `git diff` | You can restrict this to a single file by adding the filename: `git diff [FILE]`|
| View the differences between two versions | `git diff [VERSION1] [VERSION2]` | `[VERSION1]` and `[VERSION2]` are commit hashes or `HEAD` (last commit), `HEAD~1` (one commit before the last), etc. As with the other `diff` commands, you can restrict this to a single file by adding the filename. |
| View staged differences | `git diff --staged` | As with the other `diff` commands, you can restrict this to a single file by adding the filename. |
| Restore a file to its last committed version | `git restore [FILE]` | Git reminds you of this command when you use `git status`. You will **permanently** lose your uncommitted changes! |
| Unstage a file (without losing your changes) | `git restore --staged [FILE]` | Git reminds you of this command when you use `git status`. Note the different behavior from plain `git restore`: you don't lose your changes! |
| Restore a file to a specific version | `git checkout [VERSION] -- [FILE]` | `[VERSION]` is a commit hash or `HEAD`, `HEAD~1`, etc. Don't forget the spaces before and after `--`. As with `restore`, if you don't commit your current changes, you will **permanently** lose them. |
| Push your changes to the remote repository | `git push` ||
| Pull changes from the remote repository | `git pull` ||
| Clone a repository from the remote | `git clone [URL]` | `[URL]` is the remote URL of the repository (get this from the GitHub website). By default, the folder has the same name as the repository, but you can change this: `git clone [URL] [FOLDER]`|
| Delete a file from the repository | `git rm [FILE]` | Short for **r**e**m**ove. **Don't** confuse this with plain `rm`, which permanently deletes!|
| Move or rename a file in the repository | `git mv [FILE] [NEWPATH]` | Short for **m**o**v**e. **Don't** confuse this with `mv`!|
| Prevent files from being staged/committed | (add a line in the `.gitignore` file) ||


## Resources

Here is a short list of additional resources you might find useful on your Git journey:
- [***Pro Git,***](https://git-scm.com/book/en/v2) **the official Git book:** a comprehensive and free guide to Git. This is an incredibly powerful piece of software and our tutorial today only covered (roughly) one and a half of ten chapters (section "2.5 Working with Remotes"). In particular, you will probably need to learn to use `merge` at some point; I recommend skimming the relevant section.
- [**GitHub Guides:**](https://guides.github.com/) a series of official tutorials from GitHub covering Git and the specifics of GitHub.
- [**My own blog post**](https://joe-wan.github.io/blog/unix-command-line/) on UNIX Command Line for the Molecular Ecologist: basics of the UNIX command line (this includes macOS, but on Windows you might have to install a tool like [Git Bash](https://git-scm.com/download/win) to use the commands).
- **Graphical user interfaces (GUIs) for Git:** there are some standalone programs that allow you to pull, stage, commit, push, and much more with a visual interface. I recommend learning the basics on command line first: these are universal, and sometimes you will be on a system where you can't use your favorite GUI. But many may prefer a GUI program for their daily use: popular ones include GitHub's official tool [GitHub Desktop](https://github.com/apps/desktop), [GitKraken](https://www.gitkraken.com/lp/e3), and [Sourcetree](https://www.sourcetreeapp.com/).
- [***Happy Git and GitHub for the useR:***](https://happygitwithr.com/) An online book which covers how to integrate Git/R Markdown/RStudio.
- [**Integrating with RStudio:**](https://resources.github.com/github-and-rstudio/) If you primarily use R and prefer to use RStudio, you may find it convenient to manage your repository directly in RStudio; here's a guide from GitHub on how to use the built-in features. Again, I recommend learning the basics on the command line first, but some may prefer this for everyday use.
- [**Visual Studio Code**](https://code.visualstudio.com/): probably the most popular all-purpose code editor these days, for those looking for an alternative to RStudio. It has a built-in Git interface and can be customized for many languages (I use it for R!).

## Parting words

Git is only a tool! Tools are meant to make your work easier: hopefully you will find that Git helps protects you from losing your hard work and helps you write better analyses, more quickly. I've given some opinionated tips, based on others' advice and my own experience, that I think can help you achieve those aims... but don't lose sight of the bigger picture. It's up to *you* to find out how tools like Git can help *you* do good science, and have a good time doing it. Good luck!