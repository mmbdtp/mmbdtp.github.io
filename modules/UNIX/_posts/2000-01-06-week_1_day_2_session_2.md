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


