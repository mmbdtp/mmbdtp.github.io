---
title: File Permissions in Unix
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
