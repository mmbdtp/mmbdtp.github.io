---
title: Unix and the Command Line
---

##  Monday morning: Unix and the Command Line

Welcome to an introductory tutorial on usimg the Unix command line on your MacBooks. The command line is a powerful tool, enabling users to perform tasks more efficiently and automate processes. In this tutorial, we will explore the basics of navigating the file structure, creating and deleting files and directories, and some essential Unix commands.

---


### Opening the Terminal App

Click on the magnifying glass icon (Spotlight) in the top-right corner of your screen.

Type `Terminal` and press `Enter.`

Alternatively, you can open the *Applications* folder, then *Utilities* and double-click on *Terminal*.

- After you have opened the Terminal, if you mouse over the icon for the Terminal in the Dock and select _Options_, you can tell MacOS to keep the icon available in your Dock.

You should now see a window with a command-line prompt.


```
computername:~ username$

```
The command-line prompt typically displays the name of your computer, the current working directory and your username. It ends with a `$` sign, waiting for you to enter a command. 

When you first open the Terminal app, you are likely to find yourself in your home directory associated with your username and account on the MacBook. The tilde sign ~ represents a shortcut designation for that directory within Unix file systems

The terms `terminal` and `shell` are often used interchangeably, but they refer to different things.

* `Terminal`: This is the actual interface or program that opens a window and lets you interact with the shell. The terminal displays input and output. It's the graphical or text-based application you run, like the Terminal app on macOS or programs like iTerm2.
* `Shell`: The shell is the environment or command-line interpreter that processes commands. When you type commands into a terminal, it's the shell that reads, interprets, and executes those commands. There are various shells available, with `bash` being one of the most common, especially on Linux. You will see lots of online material mentioning the `bash` shell. Others include `sh, zsh, csh`, and `fish`.

When you're using the Terminal app on macOS, you're typically using the z shell `zsh` within that terminal to execute commands. If you need to switch to another shell, say `bash`, you type its name. 

Let's switch to `bash` and then back to `zsh`. 


```
bash
```

```
zsh
```

A few links

* [https://en.wikipedia.org/wiki/Unix_shell](https://en.wikipedia.org/wiki/Unix_shell)
* [https://en.wikipedia.org/wiki/Bash_(Unix_shell)](https://en.wikipedia.org/wiki/Bash_(Unix_shell))
* [https://en.wikipedia.org/wiki/Z_shell](https://en.wikipedia.org/wiki/Z_shell])

---


### Navigation: Finder vs Terminal

- **Finder** : You navigate through files and folders by clicking on them.
- **Terminal** : You navigate by typing commands then pressing the Return key

Navigate to your home directory and then to the Desktop directory using the Finder. Now let's navigate to the Desktop on the Terminal and create some directories and files there.

```
cd Desktop

```
In this command `cd` (short for "change directory") changes your current directory to the Desktop directory. You type the command, then a space and then the name of the file or directory you want the command to act on. This is how most Unix commands work. If you want to know more about how a command works, you can usually type `man` and then the command name.

Almost all Unix commands also have Wikipedia entries.

- [https://en.wikipedia.org/wiki/Cd_(command)](https://en.wikipedia.org/wiki/Cd_(command))
- [https://en.wikipedia.org/wiki/List_of_Unix_commands](https://en.wikipedia.org/wiki/List_of_Unix_commands)


Here's a cheat sheet of UNIX commands from Fosswire: 
- [Unix/Linux Command Cheat Sheet](https://files.fosswire.com/2007/08/fwunixref.pdf)

ChatGPT will also give good answers to well posed questions about what a Unix command does and how to get the best out of it.

---


### Making directories and files
Let's make a new directory in the Desktop folder

```
mkdir bioinfo_adventures

```
The `mkdir` command is used to create a new directory. You type the command, then a space and then the name of the directory you want to create, which here we are calling `bioinfo_adventures`.

Take a fresh look at the Desktop folder using the Finder and you will see this new empty directory as a folder there.

Let's move inside that new directory.

```
cd bioinfo_adventures

```
Let's use the `Touch` command to create an empty file

Look at it on the Finder. Click on it. You will see it is empty.

```
touch grandeur.txt

```
Let's add some text.

```
echo "There is grandeur in this view of life" > grandeur.txt

```
`echo` makes the shell return the text as an output and the `>` redirects that output into `grandeur.txt.` 

- See [https://en.wikipedia.org/wiki/Redirection_(computing)](https://en.wikipedia.org/wiki/Redirection_(computing))

Click on the `grandeur.txt` file in the Finder and you can see it now contains the text.

---

### Naming Conventions: Finder vs Terminal 

- **Finder**: allows filenames with spaces. Not case sensitive.
- **Terminal**: Creates difficulties for filenames without spaces; instead we tend to use underscores or quotes. Case-sensitive

Let's see what happens when we make a directory with a name containing spaces.


```
mkdir "bioinfo fun"

```
Putting quotes around the name allows us to create a file or directory name that contains spaces

- Warning: these have to be straight quotes! Microsoft often corrects these to curly quotes, so be careful when pasting from Word documents. 

An alternative approach is to escape the space using a backslash `\`

```
mkdir bioinfo\ fun

```
If you type those two commands one after another, you will get an error message saying `File exists`. 

- Sometimes Unix gives this kind of useful feedback, sometimes it does not!

If you look via the Finder you will see the directory `bioinfo fun` is sitting there. But what happens when you try to move into it on the command line?

```
cd bioinfo fun

```
You get told `no such file or directory`!

To avoid this problem, we generally use the underscore character `_` to represent white spaces in Unix file names, which also means we don't need the quote marks.

```
mkdir bioinfo_fun

```

Let's get rid of the directory `bioinfo fun` with an allied command rmdir:

```
rmdir bioinfo\ fun

```
How else could you have specified that directory to the `rmdir` command?

And now let's move into `bioinfo_fun`

```
cd bioinfo_fun

```
Note that in Unix both commands and file names are generally case sensitive. macOS is forgiving if you type `Cd Bioinfo_fun` or `CD bioinfo_fun` instead of `cd bioinfo_fun` but most Unix-like systems will fail to recognise the file name if not in precisely the right case.


---

###Coffee break!
___


### Moving around
On Unix-like systems, files and directories are managed in a hierarchical structure, where files and directories sit within folders, which in turn can in turn sit within directories higher up in the hierarchy.

To find out where you are in the hierarchy use the `pwd ` command, which stands for print working directory

```
pwd

```
The result will look something like this.

```
/Users/username/Desktop/bioinfo_adventures/bioinfo_fun

```
The forward slashes list the directories down from the topmost directory, which is called the `root` directory.

Every location (file or directory) in the file system can be described either:

- as an `absolute path`, that is the full path from the root directory to the location,

for example
  
```
  /users/admin/steve
```
  
- as a `relative path`, that is the path from the current directory to the location, so if we are inside a directory called `jon,` it would be

 
```
../admin/steve
```
  

Let's clarify these concepts:

**Absolute paths** (almost) always start with /, and they are the same for everyone, no matter where they are in the file system.
- Each absolute path is crafted separating with slashes the names of the directories in the path. The final trailing slash (for directories) is optional, but it's wrong if the path is for a file.

- This is an absolute path for a directory
  
```
/users/admin/steve
```


- This is an absolute path for a file
 

```
/users/admin/list.txt
```

    
- Remember, we already noted that `~` is a shortcut for the home directory of the current user, so if "Steve" is the active user
 

```
/users/admin/steve 
```

  can be written as


``` 
~ 
```


**Relative paths** never start with / and they are different depending on where they are in the file system.
- Your current directory is represented by the dot `.`
- The parent directory is represented by `..`
- Example: this means that you want to access a file inside a directory that is two levels above your current directory

``` 
../../directory/file.txt
```

- Remember that if you don't know where you are, you can always use `pwd` to print the current directory (its output is an absolute path).

If we want to go up to the directory above us, we can use this double dot `..` shortcut


```
cd ..

```

Type that command now.

Note that how `cd` can take an absolute or relative path as a parameter. 
- If you type `cd` on its own it takes you to your home directory. 
- If you want to return to your previous directory, you can use `cd -`

**Listing files**

If we want to find out what is in a directory we use the `ls` command (short for "list")

```
ls

```
Sometimes files and directories are hidden form us by the operating system, usually if they start with a dot. To reveal all the files, we apply the flag `-a` to the command that alters its behaviour to list all files and directories. This a typical thing to do with Unix commands, where the flags are usually one or two characters prefaced by a dash.

```
ls -a
```
```
. .. bioinfo fun bioinfo_fun grandeur.txt

```
In this case, it reveals shortcuts to the directory above with two dots `..` and a single dot `.`, which is a shortcut referring to the current directory.

If you specify a directory, the ls command will list the contents of that directory.

```
ls ~/Desktop

```

- Tip: if your screen gets cluttered with outputs of previous commands, type `clear` to clear it up

```
clear

```

