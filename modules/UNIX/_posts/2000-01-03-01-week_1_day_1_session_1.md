
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
-
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

**Manipulating files**

If we want to copy a file, we use the `cp` command. Let's copy the file `grandeur.txt` to create a new file called `Darwin.txt`

```
cp grandeur.txt Darwin.txt

```
What do you see with `ls` now?

If we want to remove a file, we use the command `rm`

```
rm Darwin.txt

```
What do you see with `ls` now?

- Remember the similar command `rmdir`, which works to remove directories—but only if they are empty.

If we want to rename or move a file, we use the `mv` command. If we just issue the command and a file name, it renames the file. Try this and then see what you see with `ls`

```
mv grandeur.txt origin.txt

```

But if we specify a file path for the destination file pointing outside the current directory, the file is moved rather than just renamed.

Try this

```
mv origin.txt bioinfo_fun

```
What do these command show us?

```
ls 
cd bioinfo_fun
ls

```
What has now changed?

The `cat` command will show you the contents of a text file.

```
cat origin.txt

```

---

### Shortcuts and mesages from the terminal

There are two quick shortcuts you can use to navigate around the Unix prompt. 

- If a file already exists in the place you are specifying, you can autocomplete the command. By pressing the tab button. Type `cat o` and press then the `tab` button. 
- Also, you can use the up `↑` and down `↓` arrows to take you up and down the set of previous commands, which can then be edited by using the left `←` and right `→` arrows to move along the line.


What happens if you type a non-existent command? Type this:

```
gotcha

```
You will get something like

```
gotcha: command not found

```

It's important that we read the messages that the terminals shows us. In this case it means that our system has no program (available to us) called `gotcha`. If we want to use it, we have to install it first. This is a made up example, and we don't need to install anything.

What happens if you issue a command when there is no file for it to act on?


```
cat Darwin.txt

```

---

### Downloading and searching a text file


`curl` is a command-line tool used to transfer data to or from a server using various protocols, including HTTPS, FTP, and many more. It supports a wide range of operations, from downloading files and web pages to interacting with applications. Its versatility makes it a staple tool for developers and system administrators for tasks like web content retrieval, API testing, and file transfers.

It's a very powerful yet complex  tool, but we will use it in a very simple way.

```
curl -o voyage_of_the_beagle.txt https://www.gutenberg.org/ebooks/944.txt.utf-8

```
Here's a breakdown of the command:

* `-o voyage_of_the_beagle.txt`: This tells `curl` to output the downloaded content to a file named `voyage_of_the_beagle.txt`.
* The URL is the address of the resource you want to download.
* After executing the command, the content will be saved in the file `voyage_of_the_beagle.txt` in the current directory.

We now have a text file of Darwin's *Voyage of the Beagle*.

Remember the command 'cat'? `cat` will direct the entire contents of a file to the screen. Some times files are large, or even impossibly large, and we save our sanity if we just print the first (or last) lines of a file. The `head` and `tail` commands are used for this purpose. Compare outputs of these commands


```
cat voyage_of_the_beagle.txt

```


```
head voyage_of_the_beagle.txt

```

```
tail voyage_of_the_beagle.txt

```

By default both `head` and `tail` print the first or last 10 lines of a file. You can change this behaviour by adding the `-n NUMBER` parameter, for example:


```
head -n 100 origin.txt 

```


The `grep` command is another powerful command. Let's use it in a very simple way to find all the lines in which Darwin mentions Patagonia

```
grep Patagonia voyage_of_the_beagle.txt

```
Remember we can redirect the putput of a command using `>`

```
grep Patagonia 944.txt.utf-8 > Patagonia.txt

```
- How can you find out what is in the file Patagonia.txt
- What has this command done?


---

###File Permissions in Unix


Unix-based systems, including macOS, manage file and directory access through permissions. These permissions determine who can read, write, or execute files and directories. Permissions are divided into three categories for each file: user, group, and others. For more details, check the [https://en.wikipedia.org/wiki/File-system_permissions](Wikipedia page on Unix file permissions).

The Read (R), Write (W), and Execute (X) permissions are often referred to as the "rwx" permissions. These permissions determine what actions can be performed on a file or directory by different users or groups.

* **Read** (R): When a user has read permission on a file, they can view the file's contents and copy it. For directories, read permission allows a user to list the directory's contents. Without read permission, a user can't access or see the content of a file or directory.
* **Write** (W): Write permission allows a user to modify the contents of a file or create new files within a directory. For directories, it also permits the deletion or renaming of files within that directory. Without write permission, a user can't make changes to the file or directory.
* **Execute** (X): Execute permission primarily applies to executable files or scripts. When a user has execute permission on a file, they can run it as a program. For directories, execute permission allows a user to enter and access the directory's contents. Without execute permission on a directory, even if a user has read access, they can't navigate into that directory.

These permissions are crucial for maintaining security and control over file and directory access in Unix-based systems, allowing administrators to restrict or grant specific rights to different users or groups, enhancing the overall integrity and usability of the system.

To view permissions, use the `ls -l` command, which stands for long format, displaying Unix file types, permissions, number of hard links, owner, group, size, last-modified date-time and name.

```
ls -l
```

You will get a list of files with information about them. 

Here is how it looks for one particular file on my computer. The format will be the same but details different for files on your your computer. 

```
ls -l -rw-r--r--@ 1 mksmab staff 1207982 4 Oct 11:22 voyage_of_the_beagle.txt 
```

The output from ls -l -rw-r--r--@ 1 mksmab staff 1207982 4 Oct 11:22 voyage_of_the_beagle.txt represents file information in a long format. 

Here's how to interpret it:

* `-rw-r--r--`: This part represents the file's permissions. In this example, "voyage_of_the_beagle.txt" has read and write permissions for the owner, but only read permissions for the group and others. The dashes (-) in the first and fourth positions indicate no special permissions like setuid or setgid, and no sticky bit is set.
* `1`: This number indicates the number of hard links to the file. In this case, there's only one link to the file.
* `mksmab`: This is the owner of the file, which is "mksmab."
* `staff`: This is the group associated with the file, which is "staff."
* `1207982`: The file's size in bytes. In this example, "voyage_of_the_beagle.txt" is 1,207,982 bytes in size.
* `4 Oct 11:22`: These fields represent the date and time when the file was last modified. In this case, the file was last modified on October 4th at 11:22 AM.
* `voyage_of_the_beagle.txt`: Finally, this is the filename.

This output provides comprehensive information about the file's permissions, ownership, size, modification timestamp, and more, making it a useful reference when working with files and directories in a Unix-based system.

###Changing File Permissions

**`chmod`: Changing Permissions**
To modify permissions, use the `chmod` command. Permissions are assigned using a numerical code or symbolic notation.

**Numerical Mode:**

* `4`: Read permission
* `2`: Write permission
* `1`: Execute permission

Each permission is represented by a digit, with the sum of these digits representing the permission level. For example, to give read and write permissions to the user and group, you would use chmod 664 filename.

**Symbolic Mode:**

* `u`: User
* `g`: Group
* `o`: Others
* `a`: All (user, group, and others)

**Give read and write permissions to the user and group**

*Numerical Mode:*

```
chmod 664 filename
```

*Symbolic Mode:*

```
chmod u=rw,g=rw filename
```

In this example, the 664 in numerical mode or u=rw,g=rw in symbolic mode grants read and write permissions to the user and group, while others have no permissions (o=).

**Give yourself permission to execute a file**

*Numerical Mode:*

```
chmod 700 filename
```

*Symbolic Mode:*

```
chmod u=rwx filename
```

In this example, the 700 in numerical mode or u=rwx in symbolic mode grants read, write, and execute permissions to the user, while the group and others have no permissions (g=,o=).

**Give everyone full permissions to do anything to a file**

*Numerical Mode:*

```
chmod 777 filename
```

*Symbolic Mode:*


```
chmod a=rwx filename
```

In this example, 777 in numerical mode grants read, write, and execute permissions to the user, group, and others. This setting allows anyone to do anything to the file.

Remember that it's essential to use file permissions responsibly, especially when granting extensive permissions like 777, as it can pose security risks. Always consider the implications and only apply such permissive settings when necessary.

**Creating a Shell Script**

A shell script is a text file that contains a series of commands and instructions written in a scripting language, typically designed for command-line interaction. These scripts are executed by a shell interpreter, which is often the default command-line interface  of an operating system, such as Bash on most Unix-like systems or zsh on MacOS.

Shell scripts are used to automate tasks, execute a sequence of commands, or perform system administration tasks in a more efficient and repeatable manner. They can include conditional statements, loops, variables, and other programming constructs, allowing users to create complex sequences of actions. Shell scripts are widely used for tasks like file manipulation, data processing, system maintenance, and more. They are a fundamental tool for power users, system administrators, and developers working in command-line environments.


So, let's create a shell script in zsh. If you are going to be using the command line to handle files in a given directory, it might be useful to ensure all your files have Unix-compatible names by replacing spaces with underscores in filenames. Choose a target directory on your Mac, say the Desktop and `cd` to it. You can edit the text for a shell script in any text editor (although take care with Word). The MacOS TextEdit would work, but if you want to stay on the command line, you can just `cat` the text into the file. 

```
cat > replace_spaces.sh <<EOF
#!/bin/zsh
for file in *; do
  if [ -f "$file" ]; then  # Check if it's a regular file
    newfile="${file// /_}"
    mv "$file" "$newfile"
  fi
done
EOF
```
Use ls -l to look at the file and its permissions.

**Making the Script Executable**

To make the script executable, use the chmod command:

```
chmod +x replace_spaces.sh
```


**Running the Script**

Execute the script in your current directory:

```
./replace_spaces.zsh
```

This script replaces spaces with underscores in all filenames within the current directory.

*Hang on!* **Why** do we need that `./` in front of the script name?

In Unix-like operating systems, including macOS, when you want to run a script or an executable file located in the current directory, you typically need to prefix the command with `./`. Here's why:

* **Current Directory**: By default, the system doesn't include the current directory (the directory you are in) in the list of directories searched for executable files. This is a security measure to prevent accidentally running malicious or unintended scripts or programs.

* **Path Lookup**: When you execute a command without specifying a path (e.g., script.sh), the shell looks for the command in directories listed in the system's PATH environment variable. These directories are predefined, like `/usr/bin` or `/usr/local/bin`. The current directory is not part of this search path.

* **Explicit Executio**n: To run a script or an executable in the current directory, you need to explicitly specify the path to the file. The `./` notation tells the shell to look for the file in the current directory. For example, `./script.sh` indicates that you want to run the `script.sh` file in the current directory.

By requiring `./`, Unix-like systems make it clear that you are running a command from the current directory intentionally, reducing the risk of accidentally executing a potentially harmful script with the same name as a system command. It's a safety measure to ensure that you explicitly indicate the location of the script you want to run.

**Editing Permissions Using Finder**

You can also change file permissions using the macOS Finder:

* Right-click the file or folder.
* Select "Get Info."
* Expand the "Sharing & Permissions" section.
* Click the lock icon and enter your password to make changes.
* Use the dropdown menus to adjust permissions for users and groups.

**Understanding "Root" and `sudo`**

"Root" is the superuser in Unix-based systems with unrestricted access. `sudo` (superuser do) allows authorized users to execute commands as the superuser or another user. sudo is often needed to install programs on Unix-like systems because many system-level tasks, including software installation and configuration, require administrative privileges. Be cautious when using sudo as it can modify system files. Learn more on the Wikipedia page about sudo.

For example, on the MacOS you logged in as a user don't have permission to add a file to one of the directories used by the operating system. 

Try this

```
touch /etc/test_file.txt     
```

To run a command as a superuser using sudo:

```
sudo touch /etc/test_file.txt     
```

Always exercise caution and avoid using sudo unless necessary.

**macOS Security Considerations**

macOS includes security features like Gatekeeper and SIP (System Integrity Protection) that restrict the execution of programs and scripts to ensure system integrity. You may encounter permission issues when running scripts or software downloaded from the internet. To overcome this, you can adjust security settings in the System Preferences to allow execution of specific applications and scripts. Always be vigilant about the security implications when modifying these settings to protect your system.


### Time for a break

Okay, so you are now bioinformaticians. It may not feel like it, but you have started your journey into microbial bioinformatics! Your brain has been rewired and you need a rest.

Finally, a few Unix in-jokes to end this session

How do Unix geeks sit down for lunch?

```
~$ grep food menu

```

Which command do Unix files hate to play hide-and-seek with?

- `ls`, because it always finds out where they are and what they have permission to do

Why bioinformatics students always feel nostalgic?

- Because every time they feel lost, they'd just cd ~ and find themselves back home.


You hate Unix??

Take a look at this old classic, which has a good moan about many aspects of Unix: 

- [The Unix-haters handbook](https://web.mit.edu/~simsong/www/ugh.pdf)



---

And now on to coffee, and then notebook servers and the cloud…



---

[Back to the Programme for this week](week_1__programme.md)
