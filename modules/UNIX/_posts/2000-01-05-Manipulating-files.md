---
Title: Manipulating files
---

**Monday afternoon: Manipulating files in Unix**

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

### File Permissions in Unix


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

***`chmod`: Changing Permissions**
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


---
title: Playing with text files
---


Text files and the command line
===============================

We have been working you very hard with all this stuff on Conda environments and Jupyter hubs. 

Let's now adopt a more gentle pace for this afternoon and work on the Terminal within our notebook server to look at some new commands and concepts to work with the command line, in particular for text file parsing and manipulation.

Use the JupyterLab interactive computing interface to open a new Terminal.

**Downloading our toy files**

Let's download an archive with some toy files to use in the next sections. 

```
wget "https://github.com/telatin/learn_bash/archive/refs/tags/2022.tar.gz"
```

**tar: extracting the archive**

We downloaded an archive in the .tar.gz format. `tar `is another compressed format very popular in UNIX systems. To decompress it, we use the `tar` command

```
tar -xzf 2022.tar.gz
```

Where we combine some swiches:

- -x switch to extract the archive,
-  the -z switch to decompress it,
- the -f parameter to specify the filename (immediately after).

Try typing `ls`: can you see that a new directory was created? 

Use `ls -l` to check the content of the new directory

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

To make our tutorials easier, we will now rename the directory to learn_bash:

```
mv ~/learn_bash-2022 ~/learn_bash
```

find
----

The `find` command is used to search for files in a directory. Unlike `ls`, it can search for files matching a pattern, and it can search in subdirectories.

In the Linux flavour of _find_, if you omit a path to scan, it will assume its the current directory. In the version of _find_ installed with MacOS, you need to specify the path to scan, like `find .`.

Some examples:

*   List all the files and subdirectories or a directory:

```
# Remember that [tab] just means to press Tab, to autocomplete

find ~/learn_b[tab]

```

Lots listed! Might be worth using that `clear` command.

*   List only the subdirectories:

```
# If you type `-type f` you will print only the files instead
find ~/learn_bash -type d

```

*   List only items ending by ‚'.faa‚':

```
find ~/learn_bash -name "*.faa"

```

Did you notice that find will report the absolute paths of the files?

wild characters and shell expansion
-----------------------------------

We introduced with the last command a special character, the asterisk `*`, that meant ‚'any string‚'. In find, we must surround patterns with double quotes.

We will now try to exemplify the power of shell expansion.

*   `*` matches **any string**, including the empty string. 
	*   For example, `find ~/learn_bash -name "*.faa"` will match all files ending by `.faa`, while `find ~/learn_bash -name "*gene*"` will list all files containing the string `gene`.
*   `?` matches any **single** character. 
	*   For example, `find ~/learn_bash -name "*R?.fastq.gz"` will match all files with the string `R` followed by exactly one more character, then a dot and ending with any string. Examples are ‚'sample1_R1.fastq.gz', ‚'sample2_R1.fastq.gz' etc but this woulr not work for `sample2_R11.fastq.gz`
*   `[A-Z]`, `[a-z]` or `[0-9]` matches any character in the range. 
	*   For example, `find ~/learn_bash -name "sample[0-9].*"` will match all files with the string `sample` followed by a digit, then a dot and ending with any string. Examples are again 'sample1_R1.fastq.gz', ‚'sample2_R1.fastq.gz' etc but not ‚'sample1_R11.fastq.gz', ‚'sample2_R1345.fastq.gz' etc'.
*   `{this,that}` matches any of the strings separated by commas. 
	*   For example, `find ~/learn_bash -name "*_R{1,2}.fastq.gz"`' will match all files ending by ‚'\_R1.fastq.gz‚' and ‚'\_R2.fastq.gz‚'.

When we use this syntax, the command will receive the expanded list of matching files. Try with ‚'echo‚' to see what happens:

```
cd ~/learn_bash/files/
echo LIST: *.png
cd -

```


and try to use the `ls` command with the same pattern.

```
ls -lh ~/learn_bash/files/*.png

```

Text files
-------------


The [`wc`](https://manpages.org/wc) command is used to count the number of lines, words and characters in a file. This only applies to simple text files.

```
wc ~/learn_bash/README.md

```

If we want, we can print only the lines (with `-l`), words (with `-w`) or characters (with `-c`).

How many lines are there in `~/learn_bash/files/README.md`?

head and tail
-------------

The [`head`](https://manpages.org/head) and [`tail`](https://manpages.org/tail) commands are used to print the first or last lines of a file. By default, they print the first 10 lines. In both commands we can change the number of lines with the `-n` option followed by the number of lines we want.

```
# Example:
head -n 3 ~/learn_bash/README.md

tail -n 2  ~/learn_bash/files/cars.csv 

less

```


----

A little less
-------------

In this session we introduced _wc_, _head_ and _tail_: they are all non-interactive commands. To interactively browse a file, we can use the [`less`](https://manpages.org/less) command.

```

less ~/learn_bash/files/origin.txt 

```

Basic Navigation Keys when using `less`:

* Up Arrow or k: Scroll up one line at a time.
* Down Arrow or j: Scroll down one line at a time.
* Spacebar or f: Scroll forward one page (full screen).
* b: Scroll backward one page.
* G: Go to the end of the file.
* 1G or gg: Go to the beginning of the file.
* /search_term: Search for a specific term within the file. Replace search_term with your search query.
* n: Move to the next occurrence of the search term.
* N: Move to the previous occurrence of the search term.

Try `/grandeur`

To exit `less` and return to the shell prompt, press the `q` key. This action closes the `less` pager and brings you back to your command line.

A useful option for `less` is `-S`, which will not wrap long lines, meaning that long lines will not be split. 

grep
----

The [`grep`](https://manpages.org/grep) command is used to search for a pattern in a file, and it prints the lines matching the pattern. The general syntax is ‚'grep PATTERN FILE(s)‚'.

```
grep "Darwin" ~/learn_bash/files/origin.txt

```

The pattern can be [more complicated](https://manpages.org/regexp) than a simple string, but we will not cover this here. To match a pattern insensitive to case, we can use the `-i` option.

```
grep -i "darwin" ~/learn_bash/files/origin.txt

```

Some useful options:

*   `-c` prints the number of matching lines, instead of the lines themselves.
*   `-v` prints the lines that do not match the pattern (inVert match)
*   `-n` prints the line number before the line itself.
*   `-w` matches only whole words (not substrings)

cut
---

The [`cut`](https://manpages.org/cut) command is used to extract columns from a file. The general syntax is ‚'_cut -d DELIMITER -f FIELDS FILE(s)_‚'. By default the delimiter is a tab, so you don‚Äôt need to specify

```

# Check the first two lines of the file first
head -n 2 ~/learn_bash/files/cars.csv

# Print only the columns model and cyl (1 and 3)
cut -d "," -f 1,3 ~/learn_bash/files/cars.csv

```

Extract the same columns, but from the file `~/learn_bash/files/cars.tsv` (tab-separated).

sed
---

The [`sed`](https://manpages.org/sed) command is used to search and replace text in a file. The general syntax is ‚'_sed -e ‚Äòs/OLD/NEW/g‚Äô FILE(s)_‚'.


```
# We can replace the "," with a "|"
sed 's/,/|/g' ~/learn_bash/files/cars.csv

```

Here we used an obscure syntax, which will become familiar with practice, as it‚Äôs borrowed by other tools and even programming languages: `s/SOMETHING/OTHER/g`. The `s` means ‚'substitute‚', the `g` means ‚'global‚' (i.e. replace all the occurrences.

Try to replace only the first occurrence, removing the `g`:

```
sed 's/,/---/' ~/learn_bash/files/cars.csv

```

sort
----

The [`sort`](https://manpages.org/sort) command is used to sort the lines of a file. By default, it sorts alphabetically.

```
sort ~/learn_bash/files/cars.csv
```

Some options:

*   `-t,` to specify the delimiter (in this case, a comma)
*   `-k FIELDS` to sort by the given fields (fields being a number or a set of numbers separated by commas)
*   `-n` to sort numerically (this is a switch)
*   `-r` to sort in reverse order (this is a switch)

sort -n -t, -k 2 ~/learn_bash/files/cars.csv

Typing multi-line commands
--------------------------

Sometimes our terminal can become too crammed with text, and we would like to type a command in multiple lines. This is possible with the `\` character, which tells the shell that the command continues on the next line. This can be useful to make commands clearer to read:

```
wget -O origin.txt \
 "https://raw.githubusercontent.com/telatin/learn_bash/master/files/origin.txt"

```

Note that when you type a backslash at the end of a line and then press enter, the shell will print a different promt (usually a `>`), which means that the command is not finished yet. The greater-than is not to be confused with the redirection operator.




## End of Day 1

### Additional resources (optional)
Here are some links to additional resources that you might want to explore in your own time to reinforce your learning. 

#### Video tutorials

* [Command Line Crash Course](https://www.youtube.com/watch?v=yz7nYlnXLfE)
* [Beginner's guide to the Bash terminal](https://www.youtube.com/watch?v=oxuRxtrO2Ag)
  
#### Lockdown Learning Bioinformatics-along**:

* [The linux file system](https://www.youtube.com/watch?v=P8OmVPd82NI&list=PLzfP3sCXUnxGpXB3NsG2WOk2txomLIwX6&index=2)
* [Working with Files](https://www.youtube.com/watch?v=QHmDPMgeheA&list=PLzfP3sCXUnxGpXB3NsG2WOk2txomLIwX6&index=3)
* [File manipulation](https://www.youtube.com/watch?v=_4G6kTZCP4c&list=PLzfP3sCXUnxGpXB3NsG2WOk2txomLIwX6&index=4)

#### Online tutorials



#### Learning resources
* *Happy Belly Bioinformatics*: a great website from "Astrobiomike" which includes [Unix training](https://astrobiomike.github.io/unix/)

* [Learn the linux command line with Ubuntu](https://ubuntu.com/tutorials/command-line-for-beginners#4-creating-folders-and-files) - A tutorial by Canonical (the company backing Ubuntu, the most popular linux distribution) to learn the linux command line.
* [Linux Professional Institute - Learning](https://learning.lpi.org/en/learning-materials/010-160/2/2.1/) - *Learning* is an initiative of the Linux Professional Institute (LPI) and supports you in preparing for our Linux and open source certifications. 
* [duke-hts-2018](https://people.duke.edu/~ccc14/duke-hts-2018/slides.html) - A full Linux training for beginner from Duke University. It is based on Python notebooks, which we will be covering later in the course.

[Back to the Programme for this week](week_1__programme.md)

---

