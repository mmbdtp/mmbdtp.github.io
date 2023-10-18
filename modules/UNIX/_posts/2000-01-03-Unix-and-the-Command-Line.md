---
title: Unix and the Command Line
---

##  Unix and the Command Line

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

