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


---

