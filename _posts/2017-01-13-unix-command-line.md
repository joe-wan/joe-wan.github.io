---
layout: blog-post
title: Unix Command Line for the Molecular Ecologist

mathjax: false

tags: [computing, tutorials]
---

Basic familiarity in the Unix command line opens a whole world of bioinformatics
tools and analysis opportunities. This guide introduces the most important
commands and concepts you'll need to get started.


Quick cheat sheet
-----------------

### Navigating the file system
- Change directories: `cd [PATH]`
- List files in directory: `ls` (current directory) or `ls [PATH]` (another directory)
- See file sizes in a directory: `ls -lh [PATH]`
- Print current directory: `pwd`
- Make a directory: `mkdir [PATH]`
- Remove an empty directory: `rmdir [PATH]`

### Working with files

Basic operations:

- Move/rename a file: `mv [FILE] [NEW PATH]`
- Delete a file (permanently!): `rm [FILE]`
- Delete an entire directory (permanently!): `rm -r [DIRECTORY]`
- Copy a file: `cp [FILE] [NEW PATH]`
- Copy an entire directory: `cp -r [DIRECTORY] [NEW PATH]`

Useful tricks:

- Download a file: `wget -O [OUTPUT FILE] [URL]` or `curl -o [OUTPUT FILE] [URL]` (`wget` may not be available in OS X)
- View a file: `head -[NUMBER OF LINES] [FILE]` or `less [FILE]`
- Create a blank file: `touch [FILE]`
- Edit a file: `nano [FILE]`
- Search within a file: `grep [QUERY] [FILE]` or `grep [QUERY] [FILE] -A [AFTER] -B [BEFORE]`
- Create a symbolic link to a file: `ln -s [FILE] [NEW LINK]`
- Wildcards: `*` matches any number of characters in a path; `?` matches one

### Working with programs

- General syntax: `[PROGRAM NAME OR PATH] [ARGUMENT 1] [ARGUMENT 2] [...]` (quote arguments if they have spaces)
- For help: `[PROGRAM] -h` or `[PROGRAM] --help` or `man [PROGRAM]`
- Redirecting output to file: `[PROGRAM] [ARGUMENTS] > [OUTPUT FILE]`
- Piping output to another program: `[PROGRAM 1] [ARGUMENTS] | [PROGRAM 2] [ARGUMENTS]`

- View zipped file: `zcat [FILE] | less` (`gzcat` on OS X)
- Redirect output to log: `[PROGRAM WITH OUTPUT] > [OUTPUT FILE]`
- Redirect output and errors to log: `[PROGRAM WITH OUTPUT] 2>&1 logfile.log`

Writing shell scripts:

- Begin the script with "shebang" line: `#!/bin/bash`
- Use `#` for comments
- Run with `bash [SCRIPT PATH]` or `[SCRIPT PATH]` (with `./` if in current directory)
- If running as `[SCRIPT PATH]`, use `chmod +x [SCRIPT PATH]` to set permissions


Why should I use the command line?
----------------------------------

Like the desktop interface you use on your computer every day, the command line
allows you to work with files and programs. Though the commands might feel
clunky at first, you'll find that using the command line offers several
important benefits:

- **Power.** Many tools can only be used through the command line. This is
  particularly the case for bioinformatics tools--to use many cutting-edge
  bioinformatic algorithms, you'll need to run them in the command line.
- **Flexibility.** With basic familiarity with the command line, you can
  combine different tools and automate parts of your analyses.
- **Reproducibility.** With its simple interface and the ability to save lists
  of commands as scripts, the command line is a powerful tool for reproducible
  analysis.
- **Working remotely.** If you want to run analyses remotely (for instance, on
  a more powerful machine or a computing cluster) you'll often need to work
  in the command line.


What is Unix? What is the command-line? What is `bash`?
-------------------------------------------------------

Unix is a family of operating systems which share a set of tools and a system
for organizing files and resources. Both Linux and OS X are "Unix-like"
operating systems, and the tools we'll use in this tutorial should work on both.

When using the command line, you enter commands which are interpreted by a
**shell** or command-line interpreter. The shell reads a command input by the
user, runs the action, then repeats this cycle indefinitely.

`bash` is the most commonly used shell and is often the default, so this
tutorial focuses on `bash` syntax. We'll just go over basic commands, but
`bash` has its own powerful (but clunky) programming language; I recommend
checking some more advanced features (`for` loops, `if` statements, and
variables come in handy quite often).



1\. Getting started: navigating the file system
----------------------------------------------

Let's get started.  Open up a command line terminal and type `ls`, then press
enter. You will see a list of files and folders. On my computer, I see:

```
Desktop
Documents
Downloads
Music
Pictures
Videos
```

The `ls` command (short for "list") lists files and folders in the current
directory. You can change directories with `cd` (short for "change
directories"). Enter the command `cd Desktop` (use the name of another directory
if you don't have a `Desktop` folder). Then, use  `ls` again. You should see
the same list of files.

How do we go back? Unix uses `..` to refer to the directory above the one we're
currently in. So, enter `cd ..`, then check that we're back in our starting
location by listing the files with the `ls` command.

You've seen how to do some basic navigation. Next, we'll learn how Unix uses
**paths** to specify locations. `Desktop` and `..` are both examples of paths.
As you may have seen before, a path is a list of folders, separated with `/`.
It is very important to distinguish between **absolute** and **relative** paths.

### Absolute paths

An absolute path refers unambiguously to a single location. Just as the address
"1600 Pennsylvania Ave NW, Washington, DC, USA" refers to a particular building,
an absolute path like `/home/joe/Documents/file.txt` refers to a specific
file. **Absolute paths begin with a forward slash `/` or a tilde `~`.** This is
because in Unix operating systems (including OS X and Linux), all files live
under a **root directory** called `/`.

The tilde `~` is a special character in absolute paths. Paths beginning with
`~` refer to the current user's home directory (often something like
`/home/USERNAME`). Usually, when starting a session in the command line, you
begin in your home directory.

### Relative paths

A relative path gives a location *relative* to the current location. Just as
the phrase "the person on my right" can refer to different people depending on
where the speaker is, a relative path like `Documents/file.txt` only makes
sense if you know the current directory (the **working directory**). If you are
in the directory `/home/joe/`, the path `Documents/file.txt` refers to a
file named `file.txt` inside a directory named `Documents` which is in the
current directory--that is, a file with the absolute path
`/home/joe/Documents/file.txt`. **If a path doesn't start with `/` or `~`, it
is a relative path.**

In relative paths, `..` and `.` have special meanings. The symbol `..` is very
useful; it means "the directory above." So, if we're in `/home/joe/Documents/`,
`../` refers to `/home/joe/` and `../../` refers to `/home/`. It can also show
up in the middle of paths: `Documents/../Files/` would refer to
`/home/joe/Files/`.

Paths starting with `./` refer to things inside the current folder, so
`./Documents` is the same as `Documents`. This may seem pointless, but this is
actually useful in some cases (e.g. when we run programs, as you'll see later).

With both types of path, be careful about spaces and certain other special
characters (including `&'"$<>()|"`). The interpreter treats spaces as
separators, so a path like `/home/joe/Documents/New File.txt` looks like two
separate parts: `/home/joe/Documents/New` and `File.txt`. Paths with special
characters need to be quoted: `'/home/joe/Documents/New File.txt'`. You can
also put a backslash before the special character:
`/home/joe/Documents/My\ File.txt`

One final note: if a path ends with a `/`, it must refer to a directory.
However, the reverse is not true: a path not ending in `/` might still be
a directory.

### Putting it together

Using your knowledge of paths, you can navigate the entire file system.
Usually, relative and absolute paths can be used interchangably.

Enter `pwd` (short for "print working directory"; it has nothing to do with
passwords). This tells you your current location as an absolute path; mine is
`/home/joe/`. `pwd` is useful when you forget where you currently are!

Earlier, we used `cd` to navigate one directory at a time, but it is actually
more powerful. You can give it an absolute path: try using `cd /` to go to `/`,
the root directory, and using `ls` to list files. Then, go back to the original
directory (use the absolute path previously printed by `pwd`). Practice using
`cd` with some different absolute and relative paths.

The `ls` command also has other useful abilities. You can give it a path to list
the contents of that path: for example, `ls /home/joe/Desktop`. Want to know
how big your files are? Use `ls -lh` to see this information. (Try including
a path after `ls -lh`!)

Finally, you can make directories with the `mkdir` command and remove an empty
directory with the `rmdir` command. Try this out by making a directory for your
command-line learning efforts: I put mine at `~/Documents/command_line_demo`.
Make a directory inside that directory called `junk`, then remove the directory
with `rmdir`.

### Summary: navigating the file system

- Change directories: `cd [PATH]`
- List files in directory: `ls` (current directory) or `ls [PATH]` (another directory)
- See file sizes in a directory: `ls -lh [PATH]`
- Print current directory: `pwd`
- Make a directory: `mkdir [PATH]`
- Remove an empty directory: `rmdir [PATH]`


2\. Working with files
---------------------

Now we know how to move around the filesystem. However, that isn't very useful
if it's all we can do! To do work using the command line, we have to know how
to work with files.

### Downloading and viewing files

Many of the files you work with will be downloaded from the web. It's often
handy to use the command line to download files. The `wget` tool will do this.
Let's download an ebook of _On the Origin of Species_ from the Project Gutenberg
website using the command:

    wget 'http://www.gutenberg.org/cache/epub/2009/pg2009.txt'

On OS X, you will have to use:

    curl -o pg2009.txt 'http://www.gutenberg.org/cache/epub/2009/pg2009.txt'

Is this the right file? We could open it up in Microsoft Word to see, but let's
stick to command line tools. The `cat` command prints the contents of a
text file. Let's try it:

    cat pg2009.txt

Wow, that printed the entire text of _On the Origin of Species_! There are other
ways to view this file without printing all of it. Try:

    head -10 pg2009.txt

This should print the first 10 lines of our file. Finally, for something fancy,
try the `less` viewer:

    less pg2009.txt

This lets you scroll up and down through the file, all in the command line!
Type the letter 'q' to exit.

### Moving, copying, and deleting files

Having our Darwin ebook is nice, but `pg2009.txt` isn't an informative title.
We could have used command line options to download it to a different location:
`wget -O [OUTPUT FILE] [URL]` or `curl -o [OUTPUT FILE] [URL]`, but this is a
good opportunity to learn how to rename files. Let's rename the file using the
`mv` ("move") command:

    mv pg2009.txt 'Origin of Species.txt'

Notice the single quotes around the filename! This is necessary because there
are spaces in the new path. The command
`mv pg2009.txt Origin\ of\ Species.txt` would also have worked. Also,
though we've just used `mv` to rename a file, `mv` will also move files between
directories if the new path is a directory (`test/`) or points to a location in
a different directory (`test/'Origin of Species.txt'`):

    mkdir test/
    mv 'Origin of Species.txt' test/
    # Equivalent to the following:
    # mv 'Origin of Species.txt' test/'Origin of Species.txt'

Now try your hand at moving the file back to our working directory (hint:
`./` is the relative path for the current directory).

Copying books is hard, but copying ebooks is trivial. Let's make a new copy of
Darwin's book using the `cp` (copy) command:

    cp 'Origin of Species.txt' darwin.txt

Use the `head` or `less` command to check that `darwin.txt` has the same text.

I'm getting overwhelmed by all these files; aren't you? Let's delete the extra
copy using the `rm` command:

    rm darwin.txt

Be **VERY** careful using the `rm` command. Unlike deleting things in the
Finder, `rm` doesn't send things to a trash folder. **Once you `rm` a file,
it's gone forever!** If you're nervous about this, you can install and use the
[`trash` tool for OS X](http://hasseg.org/trash/) or [`trash-cli` for some
Linux systems](https://github.com/andreafrancia/trash-cli).

`rm` will also delete entire directories if you need it to (even if there are
files inside). First, make a folder with some files in it:

    mkdir Library/
    cp 'Origin of Species.txt' Library/darwin.txt

You can check that this worked with the `ls` commands. To delete the directory,
use `rm -r` ("remove, recursive").

    rm -r Library/

Notice that `rm -r` looks like `ls -lh`: both
have a command (`rm`, `ls`) followed by some options (`-r`, `-lh`). Another
useful command is `cp -r` ("copy, recursive"), which copies an entire
directory (including any files inside it).  We'll learn more about options when
we discuss running programs.

### Other tricks for files

#### Creating files

Want to create a blank file? Do the following:

    touch file.txt

#### Editing text

Often you'll want to make small edits to text from within the terminal. There
are many tools for this but I find that `nano` is the simplest and easiest to
use. Try:

    nano 'Origin of Species.txt'

To exit, hit Ctrl + X; you'll be given options to save or discard changes. This
and other commands are at the bottom of the screen (`^X` is Ctrl + X and so on)
if you forget.

If you give `nano` the name of a file that doesn't exist yet, it will create
that file for you.

#### Search

Looking for text in a file is often useful for bioinformatics. However, if
you've ever accidentally opened a genome in Microsoft Word, you might know that
this could be painfully slow. The `grep` tool is very fast and incredibly
useful for bioinformatics! As a demo, we can find out what Darwin has to say
about fungus:

     grep 'fungus' 'Origin of Species.txt'

(The first thing after `grep` is what you're searching for; the next is the
file you're looking in. I mix up the order of these two all the time.)

Not bad! Darwin uses that word twice: once in the line "...the water or some
parasitic fungus is infinitely more numerous in..." and again in "...fungus
exceeds its allies in the above respects, it will then be..."

Even cooler: we can find the context of those lines. `-A [NUMBER]` and
`-B [NUMBER]` tell `grep` how many lines before and after a match to print. So:

    grep 'fungus' 'Origin of Species.txt' -A 10 -B 10

This shows us the only passage in the _Origin_ where Darwin discusses fungi--a
very interesting paragraph about niches and resource partitioning. (But do
closely related species actually compete more strongly?)

#### Links

One of my favorite Unix features is the ability to link files. Instead of
copying a file, you can make a **symbolic link** to the file that you can read
from and write to just like a real file. To do this, use `ln -s [PATH]` ("link,
symbolic"):

    ln -s 'Origin of Species.txt' 'symbolic_link.txt'

View the file (`head` or `less`) to see that the contents are the same. You
need to be careful with symbolic links, however. First, making changes in the
linked version will also change the original file--I have accidentally
overwritten data this way. Second, if your link uses a relative path and you
move the directory containing the link, the link will no longer point to a
valid file.

You can also make symbolic links to directories, which comes in handy pretty
often.

#### Wildcards

It can be frustrating using commands like `mv` or `rm` on many similar files.
Say we have a bunch of text files:

    cp 'Origin of Species.txt' junk1.txt
    cp 'Origin of Species.txt' junk2.txt
    cp 'Origin of Species.txt' junk03.txt
    cp 'Origin of Species.txt' more_junk.txt

We could delete each file separately, but this takes too long. Instead, we can
use the **wildcards** `*` and `?`. `*` can match any number of characters,
while `?` can match only one character. So, this command will delete only
`junk1.txt` and `junk2.txt`:

    rm junk?.txt

This command will delete `junk1.txt`, `junk2.txt`, and `junk03.txt`:

    rm junk*.txt

Finally, this command will delete all four files:

    rm *junk*.txt

This command successfully matches `junk1.txt`, etc. because `*` is allowed to
match zero characters.

It's important to understand how the shell actually processes wildcards. Under
the hood, whenever it sees part of a command with `*` or `?`, it will replace
that with a list of valid paths matching that command. So, when we typed
`rm junk*.txt`, the shell converted it to this command:

    rm junk1.txt junk2.txt junk03.txt

This is subtle, but very important to understand. Some commands won't take more
than one path, so using a wildcard won't allow you to run them on multiple
files. Also, using wildcards can lead to unexpected effects. What does the
following command do?

    mv junk?.txt

`mv` doesn't work with one path, but this command actually does something. Why?
The shell expands it to `mv junk1.txt junk2.txt`, and **overwrites** `junk2.txt`
with `junk1.txt`. I've accidentally overwritten data this way!

### Summary: working with files

Basic operations:

- Move/rename a file: `mv [FILE] [NEW PATH]`
- Delete a file (permanently!): `rm [FILE]`
- Delete an entire directory (permanently!): `rm -r [DIRECTORY]`
- Copy a file: `cp [FILE] [NEW PATH]`
- Copy an entire directory: `cp -r [DIRECTORY] [NEW PATH]`

Useful tricks:

- Download a file: `wget -O [OUTPUT FILE] [URL]` or `curl -o [OUTPUT FILE] [URL]` (`wget` may not be available in OS X)
- View a file: `head -[NUMBER OF LINES] [FILE]` or `less [FILE]`
- Create a blank file: `touch [FILE]`
- Edit a file: `nano [FILE]`
- Search within a file: `grep [QUERY] [FILE]`
- Create a symbolic link to a file: `ln -s [FILE] [NEW LINK]`
- Wildcards: `*` matches any number of characters in a path; `?` matches one


3\. Working with programs
------------------------

Okay, now we have some familiarity with moving around in directories and with
manipulating files. Now, we're ready to start actually doing stuff--and for
that, we need programs. We'll learn to give input and options to programs, and
to work with output.

To run a program in the command line, you type the name of the program, or a
path to the executable file that runs the program. After that, include any
arguments (information about what to do) that the program expects. All parts
of this command should be separated with spaces--this is why you had to quote
the filename if it contained spaces (to prevent it being interpreted as
multiple arguments). When we run
`grep 'fungus' 'Origin of Species.txt'`, Unix interprets this as three
parts: `grep` `fungus` `Origin of Species.txt`; `grep` is the program and
the other two parts are arguments.

### Figuring out program usage

Many of the commands we've seen take arguments that modify their behavior--
think `ls -lh` and `cp -r`. Often, these arguments ("flags") begin with one or
two hyphens. When using a new program, we have to figure out which flags we
need to do the task we want.

Fortunately, command-line programs worth their salt come with built-in help.
This can usually be found with `[PROGRAM] -h` or `[PROGRAM] --help`, or by
entering `man [PROGRAM]` (for "manual").

We'll use the `wc` command as an example. Let's say we're interested in how
many lines are in our `Origin of Species.txt` file. The `wc` ("word
count") program can count lines, but how? Let's read the help. Try the three
commands and see which one works:

    wc -h
    wc --help
    man wc

Which option gives us the line count? Count the lines using this option. You
should come up with around twenty thousand lines!


### Redirecting output

Many programs can take their input from other programs and give their output to
other programs. You can take advantage of this to control where output goes.

We saw that `grep` can be used to be search lines. Let's say we'd like to write
the fungus passage we found earlier to a file. For this we use `>`, which
**redirects** output that would be written to the command line to a file:

    grep 'fungus' 'Origin of Species.txt' -A 10 -B 10 > 'darwin_on_fungi.txt'

Run the command and check this file. Redirecting can be useful to save error
messages to a log file. For this, you'll want to use `2>&1`, which covers
error messages as well as regular output:

    [PROGRAM WITH OUTPUT] 2>&1 logfile.log

You can also send the output of programs through other programs using a `|`,
which is known as **piping** output. Let's use the `wc` program to count the
lines in the passage, without having to save it to a file:

    grep 'fungus' 'Origin of Species.txt' -A 10 -B 10 | wc -l

You can pipe multiple times and even combine with redirecting:

    grep 'fungus' 'Origin of Species.txt' | wc -l > 'darwin_on_fungi_line_count.txt'

This finds the lines, counts them, then writes the number to a file.

This is one of the most powerful features of the Unix command line. One of the
most useful commands when processing nucleotide data is `zcat [FILE] | wc -l`,
(`gzcat` if on OS X) which uses `zcat` to uncompress the data in a file, then
sends it to `wc -l` to count the lines. You can also use `zcat [FILE] | less`
to view a zipped file without uncompressing it.

### Your own programs: simple shell scripts

You can also run custom programs. One useful example is storing your command
line commands in a file called a **shell script**.

Let's say we want some easier way to run the command `ls -lh ~/` (checking
contents of home directory and sizes). Create a file called `check_home.sh` in
you working directory and edit it (using a text editor, or the `nano` command
for extra credit). Enter the following text:

    #!/bin/bash
    ls -lh ~/

The first line is a standard way to start bash scripts, called a "shebang" line
("hash" for `#` + "bang" for `!`). The part after `#!` is the program that
should be used to run the file, in this case bash (the program that processes
command line input).

Save the file and run it like this:

    bash check_home.sh

This should list information about the file in your home directory. But let's
say we want to run the `check_home.sh` program without having to type `bash`
before it. Let's try

    check_home.sh

This fails, saying `check_home.sh: command not found`. This is because Unix
won't run a program in your current directory unless you add `./` before the
name. This is a security feature: imagine if someone tricked you into
downloading a file called `cd`. So, let's try:

    ./check_home.sh

Oh no, another error: `bash: ./check_home.sh: Permission denied`. What
happened? Unix won't run a file unless you give that file **execute
permission**. This is another security feature. The `chmod` command can be used
to add this permission:

    chmod +x check_home.sh

Now, it should work as intended! Run it again using:

    ./check_home.sh

Finally, let's say we want to add information about what the script's doing.
Update the file so it says:

    #!/bin/bash
    # List the contents of our home directory
    ls -lh ~/

The second line beginning with `#` is a **comment**; it tells bash not to run
the rest of the line. Comments can also go at the end of a line of code:

    #!/bin/bash
    # List the contents of our home directory
    ls -lh ~/ # This line has a end comment

Run the commented script to see that it works properly. Comments are a great
way to make your shell scripts clearer.

You've saved the commands for a simple "analysis" into a shell script! This
is really convenient for doing complex analyses: just put all the commands into
a shell script, and run the whole script.

Bash (this particular type of shell script) is actually its own programming
language, and has variables, loops, and other features you'd expect. You can
implement complex logic in your scripts if you need.

### Summary: working with programs

- General syntax: `[PROGRAM NAME OR PATH] [ARGUMENT 1] [ARGUMENT 2] [...]` (quote arguments if they have spaces)
- For help: `[PROGRAM] -h` or `[PROGRAM] --help` or `man [PROGRAM]`
- Redirecting output to file: `[PROGRAM] [ARGUMENTS] > [OUTPUT FILE]`
- Piping output to another program: `[PROGRAM 1] [ARGUMENTS] | [PROGRAM 2] [ARGUMENTS]`

Useful pipes/redirects:

- Count lines in zipped file: `zcat [FILE] | wc -l` (`gzcat` on OS X)
- View zipped file: `zcat [FILE] | less` (`gzcat` on OS X)
- Redirect output to log: `[PROGRAM WITH OUTPUT] > [OUTPUT FILE]`
- Redirect output and errors to log: `[PROGRAM WITH OUTPUT] 2>&1 logfile.log`

Writing shell scripts:

- Begin the script with "shebang" line: `#!/bin/bash`
- Use `#` for comments
- Run with `bash [SCRIPT PATH]` or `[SCRIPT PATH]` (with `./` if in current directory)
- If running as `[SCRIPT PATH]`, use `chmod +x [SCRIPT PATH]` to set permissions
