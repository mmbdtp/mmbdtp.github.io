---
title: Monday - Session 1
---

# Bash First Steps

>**Things covered here:**
>
> - What is the terminal
> - File system navigation
> - Basic bash commands
> - Using and understanding command flags

## Introductions

Andrea, Judit and Sam are your instructors this week. We are all bioinformaticians in the QIB Core Bioinformatics Team.

Please take this time to check access to [CLIMB notebooks](https://bryn.climb.ac.uk/user/login/).

## A first look at the terminal
Open the Terminal emulator (on a Mac it is called “Terminal”, on Linux it is called “Terminal” or “GNOME Terminal”).

| Term |	What it is |
| ----- | ---------- |
| command line/terminal	| a text-based environment capable of taking input and providing output |
| shell | The language/interpreter that converts you typed commands into something the operating system can action |
| bash	| the most common shell programming language used at a Unix command-line |
| Unix	| a family of operating systems (we also use the term “Unix-like” because one of the most popular operating systems derived from Unix is specifically named as not being Unix) |
|||


:warning: We are using Mac and MacOS. MacOS is a “Unix-like” operating system (meaning it is similar but not identical to Unix). A difference between MacOS and most Unix operating systems is that by default MacOS terminals use zshell instead of bash as its shell.

## My first command

The string telatin@N121515:~$ (or similar) is called the prompt, and it’s an indication the terminal is waiting for your input. You can type a command and press Enter to execute it. If the program returns some text, it will be printed after the prompt, and when the program finishes the execution, you will receive a new prompt.

:bulb: Try yourself, type pwd and press Enter to see what it does.

``` 
pwd
```

## Navigating the file system

The file system is the way the computer organizes the files and directories (folders) on the disk. On your laptop you use a graphical interface to navigate the file system. These GUIs are curated to show common locations and hide sensitive (or dangerous locations).

In UNIX systems, all the possible directories we have access to, are nested in a single root directory, called /. The root directory is the top of the tree, and all the other directories are nested inside it (good, bad, and ugly).

## Exploring the Unix file system with ls
The ls command lists the contents of a directory or a set of files. Lets look as the folders within the root directory:

```
ls /
```

:question: Can you find the directory which contains the Applications installed with the MacOS System by default (i.e. not the Applications you may have installed).

## Moving between folders with cd

To change our current directory we can use the cd (change directory) command. First, lets return to the pwd call again:

```
pwd
```

As you can see, your home directory is a child of the /Users directory

```
cd /Users
ls
```

To return back, we can use a special dot character '.' which allows you to reference files/folders in the current directory rather than write the full path from root.

```
cd ./<username>
ls
```
Another useful character is the double dot '..' which allows you to reference files/folders in the parent directory. This can be chained together:

```
ls 
ls ..
ls ../..
```

A third useful character is the tilde '~' which allows you to reference your home directory wherever you are:
```
cd /
ls
cd ~
ls
pwd
``` 

## Hierarchical file system

Every location (file or directory) in the file system can be described either:

- as an absolute path, that is the full path from the root directory to the location, for example /Users/Shared
- or as a relative path, that is the path from the current directory to the location, so if we are inside our home directory, it would be ../Shared

Let’s clarify these concepts:

Absolute paths (almost) always start with /, and they are the same for everyone, no matter where they are in the file system.
Each absolute path is crafted by separating the names of the directories in the path with slashes. The final trailing slash (for directories) is optional, but it’s wrong if the path is for a file.

- Example: /Users/steve is an absolute path for a directory, and equals to /Users/steve/
- Example: /Users/steve/list.txt is an absolute path for a file.

Relative paths never start with /, and they are different depending on where they are in the file system.
- Example: Your current directory is represented by the dot .
- Example: The parent directory is represented by .. 
- Example: ../../directory/file.txt means that you want to access a file inside a directory that is two levels “above” your current directory.

## Expanding command functionality

The ls command lists the contents of a directory or a set of files. It’s a powerful command, and it has many options. To activate the optional functionality of a bash command you pass flags alongside the command call e.g.
```
ls -l
```
-l is a switch to print the contents in a long format, with more information. E. g. ls -l

```
ls -h
```
-h is a switch to print the file sizes in a human readable format. E. g. ls -h

```
ls -hl
```
Multiple switches can be combined. E. g. ls -lh

## Understanding command functionality

### man
UNIX comes with massive documentation. The man command (for “manual”) will open a dedicated documentation page for many of bash commands in your system. Although, this does depend on the age and origin of the command.

Let’s try opening the manual for ls:

```
man ls
```
Now you see a full page text, and you didn’t get your prompt back: this is because you are inside an interactive program. You can use the arrows to scroll and:

- g to go to the top of the page
- G to go to the bottom of the page
- / to start searching for a string (that you have to type followed by Enter)
- n to go to the next occurrence of the string you are searching for
- N to go to the previous occurrence of the string you are searching for
- q to quit the program (you will get your prompt back)

:question: Can you try - using man - to find out how to sort the output of ls by modification date or by size? (yes, you can Google if man doesn’t help you)

## Some more commands

### mkdir
To create a new directory we can use the mkdir (make directory) command. It takes a path as a parameter.

```
# Try repeating the command twice!
mkdir new_dir

ls -l

# Let's try again, will this work?
mkdir new_dir
```

### rmdir
To remove a empty directory we can use the rmdir (remove directory) command. It takes a path as a parameter.

```
rmdir new_dir
```

### touch
To quickly create a file (e.g. to test we have write permissions) we can use the touch command

```
touch new_file
```

### cp
Copy 'cp' allows you to duplicate a file

```
cp new_file new_file_2
```

cp can also duplicate folders if the -r flag 

### rm
To remove a file we can use the rm command

```
rm new_file
```
rm has several flags that are useful but dangerous

```
# The dreaded command. DO NOT RUN! 
r m -fr /
```

### mv
Move 'mv' is a cp command followed by a rm command
```
mv new_file old_file
```

### gotcha!
If we type:


```
gotcha
```
we will (very likely) get an error message like:

```
bash: gotcha: command not found
```

It’s important that we start reading the messages that the terminals shows us. In this case it means that our system has no program (available to us) called gotcha.

### echo
The echo command prints its arguments back to the standard output. We will use this as a diagnostic tool.

```
echo "Hello's world!"
```

We used double quotes around the messages. Try replacing the double quotes with single quotes in the last command, and see what happens.



## Downloading our toy files
The goal of this section is to download an archive with some toy files to use in the next sections. There are two main tools to download files from the terminal: wget and curl. We will cover both as sometimes only one is available.

:exclamation: Go to your home directory before following this section, with cd ~.

### curl
The curl command is a tool to download files from the web. The default behaviour of curl is to print the content of the file (which we don’t want), so we need to add the -o output-destination parameter. In addition we will also need to add the -L switch to follow redirects, or our file will not be downloaded.

```
curl -L -o 2022.tar.gz "https://github.com/telatin/learn_bash/archive/refs/tags/2022.tar.gz"
```

:bulb: it’s good practice, when pasting URLs from the web, to enclose them in quotes to avoid problems: if they contains characters like & or ? they will be interpreted by the shell and the command will fail.

### tar: extracting the archive
We downloaded an archive in the .tar.gz format. This is a compressed archive, and it’s very popular in UNIX systems. To decompress it:

```
tar -xzf 2022.tar.gz
```

Where we combined some switches:

- -x switch to extract the archive,
- the -z switch to decompress it,
- and the -f parameter to specify the filename (immediately after).

Try typing ls: can you see that a new directory was created? To see it’s content you can type:
```
ls -l learn_bash-2022 
```
It’s output will be something like:
```
total 16
-rw-r--r--   1 telatin  bioinfo  6660 26 Oct 12:30 README.md
drwxr-xr-x   6 telatin  bioinfo   192 26 Oct 12:30 _readme_maker
drwxr-xr-x   5 telatin  bioinfo   160 26 Oct 12:30 archives
drwxr-xr-x  18 telatin  bioinfo   576 26 Oct 12:30 files
drwxr-xr-x   7 telatin  bioinfo   224 26 Oct 12:30 misc
drwxr-xr-x  21 telatin  bioinfo   672 26 Oct 12:30 phage
drwxr-xr-x  13 telatin  bioinfo   416 26 Oct 12:30 scripts
```

The first column lists the permissions of the file and its type: if the first char is d it’s a directory, if it’s - it’s a file, if it’s l it’s a link (“shortcut” as called in Windows).

Two columns specify the owner and the group of the file. We just created them expanding an archive, so these files are owned by us.

We have a column specifying the size of the file, and the last column is the name of the file.

:exclamation: To make our tutorials easier, we will now rename the directory to learn_bash:

```
mv ~/learn_bash-2022 ~/learn_bash
```

#### Acknowledgements 

This training course was adapted from the [Happy Belly Bioinformatics Unix Course](https://astrobiomike.github.io/unix/).
