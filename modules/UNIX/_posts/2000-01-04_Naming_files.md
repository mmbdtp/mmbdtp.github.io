---
title: Naming files
---

## Naming Conventions: Finder vs Terminal 

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

