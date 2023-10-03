---
title: Introduction to Unix and the Command Line
---

## Introduction to Unix and the Command Line

Welcome to an introductory tutorial on usimg the Unix command line on your MacBooks. The command line is a powerful tool, enabling users to perform tasks more efficiently and automate processes. In this tutorial, we will explore the basics of navigating the file structure, creating and deleting files and directories, and some essential Unix commands.

---


### Opening the Terminal App

Click on the magnifying glass icon (Spotlight) in the top-right corner of your screen.

Type **Terminal** and press **Enter.**

You should now see a window with a command-line prompt.

```
computername:~ username$
```
The command-line prompt typically displays the name of your computer, the current working directory and your username. It ends with a **$** sign, waiting for you to enter a command. When you first open the Terminal app, you are likely to find yourself in your home directory associated with your username and account on the MacBook. The tilde sign ~ represents a shortcut designation for that directory within Unix file systems

---


### Navigation: Finder vs Terminal

- **Finder** : You navigate through files and folders by clicking on them.
- **Terminal** : You navigate by typing commands then pressing the Return key

Navigate to your home directory and then to the Desktop directory using the Finder. Now let's navigate to the Desktop on the Terminal and create some directories and files there.
```
cd Desktop
```
In this command **cd** (short for "change directory") changes your current directory to the Desktop directory. You type the command, then a space and then the name of the file or directory you want the command to act on. This is how most Unix commands work. If you want to know more about how a command works, you can usually type **help** and then the command name.
```
help cd

cd: cd [-L|-P] [dir]

Change the current directory to DIR. The variable $HOME is the
default DIR. The variable CDPATH defines the search path for
the directory containing DIR. Alternative directory names in CDPATH
are separated by a colon (:). A null directory name is the same as
the current directory, i.e. `.'. If DIR begins with a slash (/),
then CDPATH is not used. If the directory is not found, and the
shell option `cdable_vars' is set, then try the word as a variable
name. If that variable has a value, then cd to the value of that
variable.
```
Almost all Unix commands also have Wikipedia entries.

- [https://en.wikipedia.org/wiki/Cd_(command)](https://en.wikipedia.org/wiki/Cd_(command))

- [https://en.wikipedia.org/wiki/List_of_Unix_commands](https://en.wikipedia.org/wiki/List_of_Unix_commands)

---


### Making directories and files
Let's make a new directory in the Desktop folder
```
mkdir bioinfo_adventures
```
The **mkdir** command is used to create a new directory. You type the command, then a space and then the name of the directory you want to create, which here we are calling **bioinfo_adventures**.

Take a fresh look at the Desktop folder using the Finder and you will see this new empty directory as a folder there.

Let's move inside that new directory.
```
cd bioinfo_adventures
```
Let's use the Touch command to create an empty file

Look at it on the Finder. Click on it. You will see it is empty.
```
touch grandeur.txt
```
Let's add some text.
```
echo "There is grandeur in this view of life" \> grandeur.txt
```
**echo** makes the shell return the text as an output and the "\>" redirects that output into **grandeur.txt.** 
- See [https://en.wikipedia.org/wiki/Redirection_(computing] (https://en.wikipedia.org/wiki/Redirection_(computing))

Click on the **grandeur.txt** file in the Finder and you can see it now contains the text.

---


### Naming Conventions: Finder vs Terminal ### 

- **Finder:** allows filenames with spaces.
- **Terminal** : Creates difficulties for filenames without spaces; instead we tend to use underscores or quotes.

Let's try to make a directory with a name containing spaces.

```
mkdir "bioinfo fun"
```
Putting quotes around the name will allow you to create a file or directory name with whitespace in—but these have to be straight quotes! Microsoft often corrects these to curly quotes, so be careful when pasting from Word documents. An alternative approach is to escape the space using a backslash (\)
```
mkdir bioinfo\ fun
```
If you type those two commands one after another, you will get an error message. Sometimes Unix gives this kind of feedback, sometimes it does not!

If you look via the Finder you will see the directory bioinfo fun is sitting there. But what happens when you try to move into it on the command line?
```
cd bioinfo fun
```
To avoid this problem, we generally use the underscore character to represent white spaces in Unix file names, which also means we don't need the quote marks.
```
mkdir bioinfo_fun
cd bioinfo_fun
```
Note that Unix commands and file names are generally case sensitive. The MacOs is forgiving if you type cd Bioinfo_fun instead of cd bioinfo_fun but most Unix-like systems will fail to recognise the file name if not on precisely the right case.

---


### Moving around
On Unix-like systems, files and directories are managed in a hierarchical structure, where files and directories sit within folders, which in turn can in turn sit within directories higher up in the hierarchy.

To find out where you are in the hierarchy use the pwd command, which stands for print working directory
```
pwd
```
The result will look something like this.
```
/Users/mksmab/Desktop/bioinfo_adventures/bioinfo_fun
```
The forward slashes list the directories down from the topmost directory, which is called the root directory.

Every location (file or directory) in the file system can be described either:

- as an absolute path, that is the full path from the root directory to the location, for example
  
```/users/admin/steve```
  
- as a relative path, that is the path from the current directory to the location, so if we are inside a directory called **jon,** it would be

```../admin/steve```



Let's clarify these concepts:

- Absolute paths (almost) always start with /, and they are the same for everyone, no matter where they are in the file system.
- Each absolute path is crafted separating with slashes the names of the directories in the path. The final trailing slash (for directories) is optional, but it's wrong if the path is for a file.
- This is an absolute path for a directory
  
   ```/users/admin/steve```


- This is an absolute path for a file
 
 ```/users/admin/list.txt```

    
- **~** is a shortcut for the home directory of the current user, so if "Steve" is the active user

  ``` /users/admin/steve ```

  can be written as
  
   ``` ~ ```


- Relative paths never start with / and they are different depending on where they are in the file system.
- Your current directory is represented by the dot .
- The parent directory is represented by ...
- Example: ../../directory/file.txt means that you want to access a file inside a directory that is two levels "above" your current directory.
- Remember that if you don't know where you are, you can always use pwd to print the current directory (its output is an absolute path).

If we want to go up to the directory above us, we can use this double dot shortcut

```
cd ..
```
Note that how **cd** can take an absolute or relative path as a parameter. 
- If you type **cd** on its own it takes you to your home directory. 
- If you want to return to your previous directory, you can use **cd -**

If we want to find out what is in a directory we use the **ls** command (short for "list")
```
ls
```
Sometimes files and directories are hidden form us by the operating system, usually if they start with a dot. To reveal all the files, we apply the flag **-a** to the command that alters its behaviour to list all files and directories. This a typical thing to do with Unix commands, where the flags are usually one or two characters prefaced by a dash.
```
ls -a
. .. bioinfo fun bioinfo_fun grandeur.txt
```
In this case, it reveals shortcuts to directory above with two dots and a single dot, which is a shortcut referring to the current directory.

If you specify a directory, the ls command will list the contents of that directory.
```
ls ~/Desktop
```
If your screen gets cluttered with outputs of previous commands, type **clear** to clear it up
```
clear
```
If we want to copy a file, we use the **cp** command. Let's copy the file **grandeur.txt** to create a new file called **Darwin.txt**
```
cp grandeur.txt Darwin.txt
```
What do you see with **ls** now?

If we want to remove a file, we use the command **rm**
```
rm Darwin.txt
```
What do you see with **ls** now?

A similar command **rmdir** works to remove directories but only if they are empty.

If we want to rename or move a file, we use the **mv** command. If we just issue the command and a file name, it renames the file. Try this and then see what you see with **ls**
```
mv grandeur.txt origin.txt
```
But of we specify a file path for the destination file pointing outside the current directory, the file is moved rather than just renamed.

Try this
```
mv origin.txt ..
```
What do these command show us?
```
ls ..
cd ..
ls
```
What has now changed?

The **cat** command will show you the contents of a text file.
```
cat origin.txt
```

---

###Shortcuts and mesages from the terminal

There are two quick shortcuts you can use to navigate around the Unix prompt. If a file already exists in the place you are specifying, you can autocomplete the command. By pressing the tab button. Type "cat o" and then the tab button. Also, you can use the up and down arrows to take you up and down the set of previous commands, which can be invoked or edited.

What happens if you issue a command when there is no file for it to act on?

```
cat voyage_of_the_beagle.txt
```
What happens if you type a non-existent command?
```
gotcha
```
You will get something like
```
gotcha: command not found
```
It's important that we start reading the messages that the terminals shows us. In this case it means that our system has no program (available to us) called **gotcha**. If we want to use it, we have to install it first. This is a made up example, and we don't need to install anything.

---

### Downloading and searching a text file

The **wget** command is a tool to download files from the web. It's a very powerful tool, but we will use it in a very simple way.
```
wget https://www.gutenberg.org/ebooks/944.txt.utf-8
```
We now have a text file of Darwin's Voyage of the Beagle.

The **grep** command is another powerful command. Let's use it in a very simple way to find all the lines in which Darwin mentions Patagonia
```
grep Patagonia 944.txt.utf-8
```
Remember we can redirect the putput of a command using "\>".
```
grep Patagonia 944.txt.utf-8 \> Patagonia.txt
```
What has this command done?


---

### Time for a break

Okay, so you are now bioinformaticians. It may not feel like it, but you have started your journey into microbial bioinformatics.

Finally, a joke to end this session

How do Unix geeks sit down for lunch?
```
~$ grep food menu
```
And now on to coffee, then notebook servers and the cloud…


---


### Additional resources
Here are some links to additional resources that you might find helpful or want to explore in your own time:

