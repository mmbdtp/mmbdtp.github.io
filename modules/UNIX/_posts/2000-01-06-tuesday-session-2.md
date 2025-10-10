---
title: Tuesday - Session 2
---

# Introduction to scripting

Instead of running commands one after another like we do when we are interactively coding at the command-line, we can put the same commands into a plain-text file and then run the file. That will execute them one after another just like if we were running them individually. That’s the basis of scripting, and it’s great both for reproducibility and for saving time 🙂

> **Things covered here:**
>
> - nano: a terminal text editor
> - What a script is
> - Adding positional arguments
> - Adding print statements
> - What a shebang is
> - Permissions


This intro page covers just the fundamentals of scripting at the command line. Like with other things, this can get much more complicated as we need it to, but the basics covered here are enough to get us up and running! This page is not meant to be a detailed dive into shell scripting or all that it encompasses. If looking for a more technical and in-depth treatment like that, [linuxconfig.org](https://linuxconfig.org/) has one [here](https://linuxconfig.org/bash-scripting-tutorial) that would be a great place to start.

## A terminal text editor: nano

It is often very useful to be able to generate new plain-text files quickly at the command line, or make some changes to an existing one. One way to do this is using a text editor that operates at the command line. Here we’re going to look at one program that does this called `nano`.

When we run the command `nano` it will open a text editor in our terminal window. If we give it a file name as a positional argument, it will open that file if it exists, or it will create it if it doesn’t. Here we’ll make a new file:

```bash
cd ~/unix_intro

nano sample_names.txt
```

When we press `return`, our environment changes to the opened text file where we can type as usual. ype in a couple of sample names, one on each line – it doesn’t matter what the names are, for example:

```bash
Sample_A
Sample_B
```

Afterwards, to save the file and exit, we need to use some of the keyboard shortcuts listed on the bottom. “WriteOut” will save our file, and the `^O` represents pressing `ctrl + o` together (it doesn’t need to be a capital “O”). This will ask us to either enter or confirm the file name, we can just press `return`. Now that it is saved, to exit we need to press `ctrl + x`.

And now our new file is in our current working directory:

```bash
ls
head sample_names.txt
```


## Our first script

First we’ll demonstrate that a script is just a bunch of lines we could otherwise run interactively, but put in a file instead. Let’s make a new directory to play in:

```bash
cd ~/unix_intro
mkdir unix-scripting
cd unix-scripting
```

Remember that if we run `date` at the command line, it will print back some temporal information according to the computer we are using. And remember that `pwd` will print the current working directory, and and piping ( `|` ) ls into `wc -l` will count how many directories and files are in the current working directory (which will currently be 0 for us at the moment):

```bash
date
pwd
ls | wc -l
```

Let's add the 3 commands we just ran to a file, with `nano`. It is convention to name a command-line script like this with the extension `.sh` for “shell”:

```bash
nano first-script.sh
```

Add those 3 lines to the file in `nano` like above, and then we can press `ctrl + x` to begin to exit `nano`, it will ask us if we want to save, so press the `y` key for yes, then hit enter to keep the filename we gave it.

> **Note:** We’re going to be doing quite a bit of opening and closing nano on this page, so remember to look back up 
> here if getting used to it still 🙂

Now we have that file, we can check its contents:

```bash
ls
cat first-script.sh
```

Just putting those 3 lines into a file, we can now run it as a script. One way we can run this script, is by giving `bash` as the command, and then the file holding the script as a positional argument:

```bash
bash first-script.sh
```

And that’s it! Notice it prints out to the terminal exactly the same way as if we ran those commands one at a time interactively like above 🙂

## Scripting with positional arguments

Variables and loops are common components of scripts. These were introduced in part 5 of the Unix Crash Course, so we are going to build off of the examples used there.

First, let’s go back into our gene_annotations directory.

```bash
cd ../gene_annotations
```

The `gene_annotations.tsv` file holds some information about ~200 genes, and genes.fa holds their DNA sequences in fasta format:

```bash
head gene_annotations.tsv
head -n 2 genes.fa
```

When working with these example files before, we were pretending to be interested in grabbing the genic sequences of genes beginning with "AT3G511" using `grep` and `cut`. Let’s do that again here if we cleaned up the file:

```bash
grep "AT3G511" gene_annotations.tsv | cut -f 4 > target_gene_ids.txt
head target_gene_ids.txt
```

Then we wrote a for loop to pull and create a file that just held those sequences of interest:

```bash
for gene in $(cat target_gene_ids.txt)
do
    grep -A 1 ">$gene" genes.fa
done > target_genes.fa

head target_genes.fa
```

As we’ve seen, we can just copy and paste the above code block into a file, and then run it as a script, and it will work the same as when we entered it interactively. But we can also make the script more flexible and more re-usable if we make it work with positional arguments, rather than needing the file that holds the target genes to be “hard-coded” into the script. Meaning, right now we have “target_gene_ids.txt” in there explicitly, and if we want to change our target genes, we either need to open the script and change that file name at that location, or we need to change what gene IDs are in the “target_gene_ids.txt” file.

Inside a script, we can access positional arguments given to the script when it’s called by referencing them as variables with their appropriate position. If not familiar with this, it can sound way more confusing in words than actually just seeing it. So let’s just start doing it 🙂

Let’s put this next codeblock into a new script called “parsing-genes.sh”, where instead of specifying the “target_gene_ids.txt” filename explicitly like before, we are putting a `$1` in it’s place:

```bash
for gene in $(cat $1)
do
    grep -A 1 ">$gene" genes.fa
done > target_genes.fa
```

That `$1` in the script will reference the first positional argument we give when we run the script. To see this in practice, first let’s remove the `target_genes.fa` output we are expecting:

```bash
rm target_genes.faa
```

And here’s how we can run it giving our “target_gene_ids.txt” file as the positional argument:

```bash
bash parsing-genes.sh target_gene_ids.txt
```

Nothing prints to the screen, but if we check, the “target_genes.faa” file holding our two genes of interest has been produced just like before:


> **Note:**
> There is not much inherently built-in to shell scripting like this that prevents problems. 
> For instance, if we try to run this without providing a positional argument, it will just hang there 
> (same as if we ran `cat` by itself with no positional argument telling it what file to act on – because that’s 
> exactly what’s happening inside the script if we don’t give it a positional argument). We can cancel that 
> with `ctrl + c`, but just keep in mind that if something will fail interactively, it will probably fail within
>  the script in the same way. We’ll point to some things we can do to help a little with cases like this below in 
> the Bash “strict mode” section.

Say we also wanted to be able to name the output file when we use the script, rather than opening it and changing it each time. We could make that a positional argument too. Let’s open up `nano` and change our “parsing-genes.sh” script again to this now, where a `$2` replaces where we are redirecting the output from the loop:

```bash
for gene in $(cat $1)
do
    grep -A 1 ">$gene" genes.fa
done > $2
```

```bash
nano parsing-genes.sh
```

Now we can specify the file holding the gene IDs we want to search for with the first positional argument we specify after `bash parsing-genes.sh` and the new output fasta file we want to create with the second positional argument:

```bash
bash parsing-genes.sh target_gene_ids.txt wanted_genes.fa
head wanted_genes.fa
```

**Let’s modify this a bit further, and make it so we can just specify which genes we want to search for, and the script will do the rest!**

To do this, we are going to need to do two things:

1. add in our `grep` and `cut` line from above that created the “target_gene_ids.txt” file based on searching for the wanted genes in the “gene_annotations.tsv”
2. name some files with variables inside the script

We’ll also add some comments that help explain what certain parts of our script are doing. Lines that are preceded by a lone `#` are ignored when the program is run, so we can add in text with those that help others (and us in the future) be able to more easily understand what our code is trying to do.

So here’s what we want in our “parsing-genes.sh” script now:

```bash
nano parsing-genes.sh
```

```bash
# generating the list of target gene IDs we want based on our search term
grep $1 gene_annotations.tsv | cut -f 4 > ${1}_target_gene_ids.txt

# looping through our target gene IDs, 
# grabbing their sequences and writing them out to a new file
for gene in $(cat ${1}_target_gene_ids.txt)
do
    grep -A 1 ">$gene" genes.fa
done > ${1}_genes.fa
```

Notice we added in braces surrounding our variables in places where they were attached to more characters, like `${1}_target_gene_ids`.txt. This is because if we didn’t do that, the `$` would be trying to find a variable with all of the characters that follow (well, up to something like the period at least), and we don’t want to look for a variable called “1_target_gene_ids”, we just want to use the “1”. So we use the braces `{}` to block out the part the `$` is supposed to be working with.

And now, we can provide *just* the gene search term we want to look for as a positional argument:

```bash
ls
bash parsing-genes.sh AT3G511
ls
head AT3G511_genes.fa
```

We can see it created 2 new files, “AT3G511_target_gene_ids.txt” and “AT3G511_genes.fa”, based on the positional argument we gave it.

So now we can use our script to search for other genes too, e.g., running:

```bash
bash parsing-genes.sh AT5G571
ls AT3G511_*
head AT3G511_genes.fa
```

Creates 2 new files based on that gene search term.


## If/then conditional
We have very few guard-rails on here to prevent weird things from happening. How much we care about that sort of thing entirely depends on how robust we want or need our little script to be. Unless we are developers, we are usually writing something to do a very specific task to help with processing or analysis that we are going to be using directly ourselves. In that case we may not need to spend any time on making a script more robust, so long as it gets our job done properly. Even for scripts like that that I write, I’ll usually put in some print statements or a check here and there that might help when we're using it.

Right now, if we search for something that doesn’t exist, the script will still finish and create two blank files:

```bash
bash parsing-genes.sh europa
ls -l europa*
```

First, let’s add a check that the target gene IDs we are looking for was actually found. There are many ways we could go about this. Here, we’ll just check that the file we create from the first searching line actually has something in it, because if it’s empty, nothing was found. We can add in an if/then statements to control part of our script. If the target_gene_ids file holds targets, we will run the for loop that pulls out the wanted sequences into a new file. If it doesn’t, we will just remove the empty file. Here’s what this looks like:

```bash
nano parsing-genes.sh
```


```bash
# generating the list of target gene IDs we want based on our gene search term
grep $1 gene_annotations.tsv | cut -f 4 > ${1}_target_gene_ids.txt

# checking any targets were found
if [ -s ${1}_target_gene_ids.txt ]
then
    # if there is stuff in the file
    # looping through our target gene IDs,
    # grabbing their sequences and writing them out to a new file
    for gene in $(cat ${1}_target_gene_ids.txt)
    do
        grep -A 1 ">$gene" genes.fa
    done > ${1}_genes.fa

else
    # if there is nothing in the file, remove it
    rm ${1}_target_gene_ids.txt
fi
```

Now, if we try it on something that isn’t found, we don’t generate any empty files:

```bash
rm europa_*
bash parsing-genes.sh europa
ls europa_*
```

## Adding some print statements

Sometimes it’s convenient to have a script print out some information for us too. Right now, this script just returns the prompt whether it found what we looked for or not. We can check afterwards, but if we wanted we can also make the program report some basic information. Let’s modify the script to be like this:

```bash
# generating the list of target gene IDs we want based on our gene search term
grep $1 gene_annotations.tsv | cut -f 4 > ${1}_target_gene_ids.txt

# checking any targets were found
if [ -s ${1}_target_gene_ids.txt ]
then
    # if there is stuff in the file
    # count how many lines (genes found)
      # this is a little ugly so that it works with standard mac and linux `wc`
    num_found=$(wc -l ${1}_target_gene_ids.txt | sed 's/^ *//' | cut -f 1 -d " ")
    printf "\n  ${num_found} genes identified!\n\n"

    # looping through our target gene IDs,
    # grabbing their sequences and writing them out to a new file
    for gene in $(cat ${1}_target_gene_ids.txt)
    do
        grep -A 1 ">$gene" genes.fa
    done > ${1}_genes.fa

else
    # if there is nothing in the file, remove it
    rm ${1}_target_gene_ids.txt
    printf "\n  No genes found with that annotation.\n\n"
fi
```

Now we get a message printed out when we run the script letting us know if it found any, and if so, how many:

```bash
bash parsing-genes.sh europa
bash parsing-genes.sh AT5G571
```


## A few additional notes

### What’s a ‘shebang’?

A [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) is something that we can put as the first line of the script, starting with `#!`, that tells the computer what to use to try to run that file as a script. The way we were running things above, specifying `bash` as the command before giving it the script as a positional argument, we didn’t need a shebang because we were explicitly telling the computer what to use. If we want to be able to run the script without doing that (or we want to call it from somewhere else in our PATH), we should add a shebang (we’ll also need to change permissions on the file, but we’ll discuss that next).

There are different ways to specify what to use, but a good, portable shebang for bash is #! `/usr/bin/env bash`, so let’s add that to the top of our script like so:

```bash
#!/usr/bin/env bash

# generating the list of target gene IDs we want based on our gene search term
grep $1 gene_annotations.tsv | cut -f 4 > ${1}_target_gene_ids.txt

# checking any targets were found
if [ -s ${1}_target_gene_ids.txt ]
then
    # if there is stuff in the file
    # count how many lines (genes found)
      # this is a little ugly so that it works with standard mac and linux `wc`
    num_found=$(wc -l ${1}_target_gene_ids.txt | sed 's/^ *//' | cut -f 1 -d " ")
    printf "\n  ${num_found} genes identified!\n\n"

    # looping through our target gene IDs,
    # grabbing their sequences and writing them out to a new file
    for gene in $(cat ${1}_target_gene_ids.txt)
    do
        grep -A 1 ">$gene" genes.fa
    done > ${1}_genes.fa

else
    # if there is nothing in the file, remove it
    rm ${1}_target_gene_ids.txt
    printf "\n  No genes found with that annotation.\n\n"
fi
```

### What about permissions?

Whether or not we need to worry about permissions also depends on how we want to be able to call the script. If we are going to call it like we initially did above, giving `bash` as the command before it, then we don’t need to change permissions on the file because it’s being “read” by the `bash` interpreter we are passing it to. But if we wanted to be able to call it without specifying the command `bash` first, then we need to change the permissions on the file to make it executable.

We can do this with the `chmod` command, for change mode, like so:

```bash
ls -l parsing-genes.sh
chmod +x parsing-genes.sh
ls -l parsing-genes.sh
```

Getting into permissions in general is beyond this page, but note that there are 3 x’s where there used to be dashes, and the color of the file changed here (which may or may not happen depending on how our environment is set up).

Now we can use the script like so:

```bash
./parsing-genes.sh AT5G571
```

Whereas if we tried that before the `chmod` command we ran, we would have gotten a permission denied error.

>**Note:**
> We needed to put the `./` in front of the script name because things that we can call as executable programs 
> like we are trying to do here need to be in our PATH. And since the current working directory isn’t in our PATH, 
> we need to explicitly point to the file’s location.

### Bash “strict mode”

As noted above, there are not a whole lot of guard-rails while writing scripts in a Unix-like shell. It’s generally not a big deal when creating ad-hoc scripts to do small things for our own analyses, but if getting into writing larger scripts, Bash strict mode might be something worth looking into. Bash strict mode is sort of an unofficial shorthand for adding things to the top of the script like `set -u` which will cause the script to exit if an undefined variable is used, and `set -e` which exits if any command doesn’t finish with its standard exit code. If you are interested in looking into this more, Aaron Maxwell has a page up on his site here that nicely walks through examples where his prefered configuration can help avoid potential problems.

And that’s the general gist of scripting at the command-line! Again, what’s here is only a brief introduction, hopefully helpful to get any newcomers up-and-running! If creating a script that we want to have available anywhere (as in, not just callable from the same directory or explicitly pointing to it), we'll need to explore what our PATH is (which we will be covering next).

#### Acknowledgements 

This training course was adapted from the [Happy Belly Bioinformatics Unix Course](https://astrobiomike.github.io/unix/).