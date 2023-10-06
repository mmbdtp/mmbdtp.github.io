Text files and the command line

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


Lots listed! Might be worth using that `clear` command.
	*   For example, `find ~/learn_bash -name "*.faa"` will match all files ending by `.faa`, while `find ~/learn_bash -name "*gene*"` will list all files containing the string `gene`.
	*   For example, `find ~/learn_bash -name "*R?.fastq.gz"` will match all files with the string `R` followed by exactly one more character, then a dot and ending with any string. Examples are ‚'sample1_R1.fastq.gz', ‚'sample2_R1.fastq.gz' etc but this woulr not work for `sample2_R11.fastq.gz`
	*   For example, `find ~/learn_bash -name "sample[0-9].*"` will match all files with the string `sample` followed by a digit, then a dot and ending with any string. Examples are again 'sample1_R1.fastq.gz', ‚'sample2_R1.fastq.gz' etc but not ‚'sample1_R11.fastq.gz', ‚'sample2_R1345.fastq.gz' etc'.
	*   For example, `find ~/learn_bash -name "*_R{1,2}.fastq.gz"`' will match all files ending by ‚'\_R1.fastq.gz‚' and ‚'\_R2.fastq.gz‚'.
-------------
-------------

In this session we introduced _wc_, _head_ and _tail_: they are all non-interactive commands. To interactively browse a file, we can use the [`less`](https://manpages.org/less) command.

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


 "https://raw.githubusercontent.com/telatin/learn_bash/master/files/origin.txt"