---
title: Monday - Session 2

---

# Redirectors and wildcards

>**Things covered here:**
>
> - Wildcards
> - Autocomplete with tab
> - Redirectors
> - History


To be sure we are still working in the same place, let’s run:

```
cd ~/unix_intro
```

## Special Characters

Bash uses special characters that have specific meanings. Some enable pattern matching (wildcards), while others help with navigation.

### The asterisk (*)

Wildcards are special characters that enable us to specify multiple items very easily. The most common one you'll encounter is `*`, so let's try it out!

As we’ve seen, `ls` lists the contents of the current working directory, and by default it assumes we want everything:

```
ls
```

But we can be more specific about what we’re interested in by giving it a positional argument that narrows things down. Let’s say we only want to look for files that end with the extension “.txt”. The `*` wildcard can help us with that.

Here’s an example:

```
ls *.txt
```

What this is saying is that no matter what comes before, if it ends with “.txt” we want it.

> At the command line, the `*` means any character, any number of times (including 0 times).

For a more practical example, let’s change directories into that messy subdirectory we saw earlier:

```
cd data/all_samples/
ls
ls | wc -l
```

So there are 900 files here, and it looks like there are 3 different extensions: “.txt”; “.tsv”, and “.fq” (a common extension for the “fastq” format, which holds sequences and their quality information).

<blockquote>
<center><strong>QUICK PRACTICE!</strong></center>

With 900 files and 3 file types (".txt", ".tsv", and ".fq"), we might expect there to be 300 of each type, but let's make sure. 
Using what we've seen above, how can we count how many files of each type there are in this directory?

<details>
<summary>Solution</summary>
<br>
<pre><code>
ls *.txt | wc -l
ls *.tsv | wc -l
ls *.fq | wc -l
</code></pre>
<p>Ah good, it's nice when things make sense 🙂</p>
</details>
</blockquote>


So far we’ve just been using the `*` wildcard with the `ls` command. But wildcards can be used with many of the common Unix commands we’ve seen so far.

For example, we can use it with the `mv` command to move all 300 of the “.fq” files into their own directory at once:

```
ls | wc -l

mkdir fastq_files
ls fastq_files/

ls *.fq
mv *.fq fastq_files/

ls fastq_files/

ls | wc -l
```

<blockquote>
<center><strong>QUICK QUESTION!</strong></center>

Why does this say 601 instead of 600?

<details>
<summary>Solution</summary>
<br>
<pre>
It's also counting the new directory we created 🙂
</pre>
</details>
</blockquote>

<br>

> Note: When using wildcards, running `ls` first like done in the above example (`ls *.fq`) is good practice before 
> actually running a command. It is a way of checking that we are specifying exactly what we think we are specifying.

## Tab completion

One of the nicest facilities of the modern shell is the built in "completion" support. These facilities allow you to complete commands and their arguments easily. Most shells allow command completion, typically bound to the TAB key, which allow you to complete the names of commands stored upon your PATH, file names, or directory names. This is typically used like so:

```
ls sa[TAB]
ls sample_[TAB]
Display all 900 possibilities? (y or n) [y]

sample_1.fq
sample_1.tsv
sample_1.txt
sample_10.fq
sample_10.tsv
sample_10.txt
sample_11.fq
sample_11.tsv
sample_11.txt
...

```

When you type `sa` and press TAB, the shell completes it to `sample_` because that matches all the files starting with `sa`. 

If you press TAB again and there are multiple possibilities, the shell shows you all the options. If there's only one match, it completes the entire filename automatically. 

Tab completion also works with commands and directories. For example, if you type `mkd[TAB]`, the shell will complete it to `mkdir` if that's the only command starting with "mkd" on your system. 

```
mkd[TAB]
mkdir 
```

Let's say you want to go back to the top level directory `unix_intro` but you're not sure how many levels up that is from where you are. You could of course print working directory and see this, or you could use tab to see the directory structure like so:

```
pwd
~/unix_intro/data/all_samples

cd ../[TAB]
all_samples/

cd ../../[TAB]
data/      example.txt     experiment/     six_commands/
```


## Redirectors

When we are talking about “redirectors” here, we are referring to things that change where the output of something is going. The first we’re going to look at is called a “pipe” (`|`). Let's make sure we have returned to our home directory by executing `cd ~`.

> A pipe (`|`) is used to connect multiple commands. It takes the output from the previous command
> and “pipes” it into the input of the following command.

Let’s look at an example. Remember we used `wc -l` to count how many lines were in a file:

```
wc -l example.txt
```

And that `ls` lists the files and directories in our current working directory:

ls
If we “pipe” (`|`) the `ls` command into the `wc -l` command, instead of printing the output from `ls` to the screen as usual, it will go into `wc -l` which will print out how many items there are:

```
ls | wc -l
```

For another example, let’s look at what’s in the subdirectory, “`data/all_samples/`”:

```
ls data/all_samples/
```

That prints out a lot of stuff, let’s see how many things are in that directory:

```
ls data/all_samples/ | wc -l
```

We’ll get back to making sense of that when we get to *wildcards* in the next section.

> Another important character is the greater than sign, `>`. This tells the command line to *redirect* 
> the output to a file, rather than just printing it to the screen as we’ve seen so far.

For an example of this we will write the output of `ls` to a new file called “`directory_contents.txt`”:

```
ls
ls > directory_contents.txt
```

Notice that when we redirect the output with the `>`, nothing printed to the screen. And we’ve just created a file called “`directory_contents.txt`”:

```
ls
head directory_contents.txt
```

**It’s important to remember that the `>` redirector will overwrite the file we are pointing to if it already exists.**

```
ls experiment/ > directory_contents.txt
head directory_contents.txt
```

If we want to append an output to a file, rather than overwrite it, we can use two of them instead, `>>`:

```
ls >> directory_contents.txt
head directory_contents.txt
```


### History

The shell also keeps track of our previous commands for us. There are a few different ways we can take advantage of this, one is by using the `history` command. But that alone will print all of it to the screen. It can be more practical to “pipe” (`|`) that into something else like `tail` to see the last few commands:

```bash
history | tail
```

Or `less` so we can scroll through our previous commands:

```bash
history | less
```

To get out of `less`, press the `q` key.

We can also use the up and down arrows at the command line to scroll through previous commands. This is useful for finding commands, but it’s also useful for making sure we are acting on the files we want to act on when using wildcards. As mentioned above, we can check first with `ls *.fq`, press `return` to see we are acting on the files we want, and then press the up arrow to bring up the previous command, and change it to what we want without altering the “*.fq” part of the command – as we already know it’s correct. Any time we can remove the chance of human error, we should 🙂

<blockquote>
<center><strong>QUICK PRACTICE!</strong></center>

We've already moved all the ".fq" files into their own directory. Create separate directories for the ".txt" files and the ".tsv" files too, and then try to move those files into their appropriate directories.

<details>
<summary>Solution</summary>
<br>
<pre><code>
mkdir text_files
ls *.txt
mv *.txt text_files

mkdir tsv_files
ls *.tsv
mv *.tsv tsv_files

ls
</code></pre>
<p>It doesn't matter what the directories are named, but at the end they should be the only 3 things in the working directory 🙂
</p>
</details>
</blockquote>



## Summary

They may seem a little abstract at first, but redirectors and wildcards are two fundamental concepts of working at the command line that help make it a very powerful environment to work in. Just knowing they exist and generally what they do means that we can learn more about them when needed 🙂

#### *Special characters introduced:*

| Character &nbsp;&nbsp;&nbsp;&nbsp; | Function                                                  |
|:-----------------------------------| ----------------------------------------------------------|
| `*` &nbsp;&nbsp;&nbsp;&nbsp;       | Represents any character appearing any number of times    |
| `?` &nbsp;&nbsp;&nbsp;&nbsp;       | Represents any character appearing only once              |
| `|` &nbsp;&nbsp;&nbsp;&nbsp;       | A "pipe" allows stringing together multiple commands      |
| `>` &nbsp;&nbsp;&nbsp;&nbsp;       | Sends output to a file (overwrites target file)           |
| `>>`&nbsp;&nbsp;&nbsp;&nbsp;       | Sends output to a file (appends to target file)           |

#### Acknowledgements 

This training course was adapted from the [Happy Belly Bioinformatics Unix Course](https://astrobiomike.github.io/unix/).

