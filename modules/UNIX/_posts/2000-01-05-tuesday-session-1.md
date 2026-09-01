---
title: Tuesday - Session 1
---

# Variables and for loops

> **Things covered here:**
> - Variables
> - For/while loops
> - Conditional logic with if/else


After our first day introducing default Bash commands, we will spend today exploring the more advanced concepts required to create our own customised scripts. In this session we will talk about bash variables, loops, and logic.

## Variables

In Bash, unlike most other programming languages, all Bash variables are untyped character variables. Most other programming languages demand you explicitly state what type of variable it is (number, string, dictionary etc.)  or it tries to infer the type dynamically as soon as you define it. Bash does not care. It is down to the command that uses the variable to determine if the variable is in anyway suitable, i.e. check it is a number before it tries to divide it.

Lets define a variable

```bash
my_var=Europa
```

Nothing prints out when a variable is set, but the value “Europa” has been stored in the variable “my_var”.

To use what’s stored in a variable, the variable name needs to be preceded by a `$` so the shell knows to *evaluate* what follows, rather than just treat it as generic characters.

To see this in action, we’ll use the `echo` command. `echo` is a command that prints out whatever is provided to it (it turns out this is actually really useful in some places – like in a program, for example, to report information to the user). Here we’ll use it to check what’s being stored in our variable:

```bash
echo $my_var
```

Note that if we don’t put the `$` in front, `echo` just prints out the text we gave it:

```bash
echo my_var
```

Recall that spaces are special characters on the command line. If we wanted to set a variable that contained spaces, we could surround it in quotations to tell Unix it should be considered as one thing:

```bash
my_new_var="Europa is awesome."

echo $my_new_var
```

```bash
# Note that calling undefined variables does not give an error!
echo $fake_var
```

### Local variables and extending scope

Like all programming languages, shell variables have different scopes. That is to say, they are only defined when called in certain functions or environments and are undefined elsewhere. The variables defined above are locally defined. They can only be used by commands called in this session. 

You can promote a variable to extend its scope to child processes of this parent shell session with the `export` command:

```bash
local_variable=here
export exported_variable=there

# both are available in this bash session
echo $local_variable
echo $exported_variable

# Only the exported variable is available if a call a new shell session
zsh -c 'echo $local_variable'
zsh -c 'echo $exported_variable'
```

This distinction may seem subtle, but can have consequences when we are calling our own scripts later on.

Both local and exported variables are deleted if the parent shell session is closed

### System vs User defined

The only meaningful distinction between shell variables are system-defined variables and user-defined variable. The above are examples of user-defined variables. They are typically ephemeral, i.e. they are deleted when you close your terminal. 

System-defined variables, as the name suggests, are persistent variables that are critical to some sub-system of the OS or shell. You can still see them and you can change them, but be warned doing so can have unexpected consequences. Restarting your shell will restore these variables to their system defaults.

```bash
# example of a system-defined variable
echo $HOME
```

We will discuss how to make user-defined variables persistent later as well as how to make changes to the system-defined variables stay too.

## For loops

Let’s make a new directory to work in:

```bash
mkdir for_loops
cd for_loops/
```

### The 4 magic words

There are 4 special words in the syntax of a For Loop in Unix languages: `for`, `in`, `do`, and `done`.

| Magic word &nbsp;&nbsp;&nbsp;&nbsp;   | Purpose                                                         |
|:-----------------------------------| -------------------------------------------------------------------|
| `for` &nbsp;&nbsp;&nbsp;&nbsp;     | set the loop variable name                                          |
| `in` &nbsp;&nbsp;&nbsp;&nbsp;      | specify whatever it is we are looping over                         |
| `do`&nbsp;&nbsp;&nbsp;&nbsp;       | specify what we want to do with each item                          |
| `done` &nbsp;&nbsp;&nbsp;&nbsp;    | tell the computer we are done telling it what to do with each item |


Let’s see what this looks like in practice. Here we are going to: name the variable “item” (we can name this whatever we want); loop over 3 words (car, truck, and ukulele); and we’re going to just `echo` each item, which will print each word to the terminal.

```bash
for item in car truck ukulele
do
  echo $item
done
```

> **Note:** Notice the prompt is different while we are within the loop syntax. This is to tell us we are not at the typical prompt. 
> If we get stuck with that alternate prompt and we want to get rid of it, we can press `ctrl + c` to cancel it.

Just to note, we don’t need to put these on separate lines, and we don’t need to indent over the “body” of the loop like we did above (the `echo $item` part), but both can help with readability so we will continue doing that moving forward. As an example though, we could also enter it like this on one line, separating the major blocks with semicolons:

```bash
for item in car truck ukulele; do echo $item; done
```

We can also do multiple things within the body of the loop (the lines between the special words `do` and `done`). Here we’ll add another line that also writes the words into a file we’ll call “words.txt”:

```bash
for item in car truck ukulele
do
  echo $item
  echo $item >> words.txt
done
```

Now we created a new file that holds these words:

```bash
ls
head words.txt
```

<blockquote>
<center><strong>QUICK QUESTION!</strong></center>

Notice that we used <code>>></code> as the redirector inside the loop, and not <code>></code>. Why do you think this is? What would have happened if we used <code>></code> at that same location?

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">for item in car truck ukulele
do
  echo $item
  echo $item > test.txt
done</code></pre>

<pre><code>head test.txt</code></pre>

<p>Since <code>></code> overwrites a file, each time we go through the loop it would overwrite the file just adding the word of the current iteration and at the end we'd be left with just the last one in our file.</p>
</details>
</blockquote>

Usually we won’t want to type out the items we’re looping over, that was just to demonstrate what’s happening. Often we will want to loop through items in a file, like a list of samples or genomes.

### Looping across files in a folder

The possible arguments accepted by the **in** of a for loop are varied making for loops very versatile. A very useful option is to loop through all folders or through all files in a folder.

First, lets create some files:

```bash
for file in file1 file2 file3
do
  touch $file
done
```

Now, lets loop through these files and rename them:

```bash
for file in file*
do
  mv $file $file_new
done
```

Notice how we did not need to explicitly list all the files. The wildcard * automatically tells the shell that the variable is a filename expansion and should look in the current directory to find which files fit the description (it is a powerful default bash utility called Globbing).

```bash
# check the output to confirm the files have changed
ls
```

```bash
# clean up after ourselves
rm file*
```

### Looping through lines of a file

We can also execute a command in such a way that the output of that command becomes the list of things we are looping over.

We’re going to use the `cat` command to help us do this (which comes from con**cat**enate). cat is kind of like head, except that instead of just printing the first lines in a file, it prints the whole thing:

```bash
cat words.txt
```

Here we’ll use `cat` to pull the items we want to loop over from the file, instead of us needing to type them out like we did above. The syntax of how to do this may seem a little odd at first, but let’s look at it and then break it down. Here is an example with our “words.txt” file we just made:

```bash
for item in $(cat words.txt)
do
  echo $item
done
```

Here, where we say `$(cat words.txt)`, the command line is performing that operation first (it’s evaluting what’s inside the parentheses, similar to what the dollar sign does when put in front of our variable name, “item”), and then puts the output in its place. We can use `echo` to see this has the same result as when we typed the items out:

```bash
echo $(cat words.txt)
```

### Retrieving specific sequences with a loop

Now imagine we want to pull out all of the gene sequences that have gene IDs beginning with "AT3G511". We can get the gene IDs using `grep` like we did previously and then using `cut` to just keep the gene ID column:

```bash
cd ~/unix_intro/gene_annotations

grep "AT3G511" gene_annotations.tsv | cut -f 4

AT3G51140
AT3G51190
```

And let’s write them to a file:

```bash
grep "AT3G511" gene_annotations.tsv | cut -f 4 > target_gene_ids.txt

ls
head target_gene_ids.txt
```

For pulling a few sequences out of a fasta file, `grep` can be very convenient. But remember the format of fasta is each entry takes two lines, and if we use grep with default settings to find a gene ID, we will only get the line with the gene ID:

```bash
grep "AT3G51190" genes.fa
```

Fortunately, `grep` has a handy parameter that let’s you pull out lines following your matched text also (in addition to just the line with the matched text), it’s the `-A` parameter. So we can tell `grep` to pull out the line that matches and the following line like so:

```bash
grep -A 1 "AT3G51190" genes.fa
```

Great! Back to our target genes, we usually won’t want to do that for every individual sequence we want (even though we only have 2 in our example here). So let’s loop through our “target_gene_ids.txt” file! Here’s just with echo like we did above, to see how we can add the `>` in front of the variable:

```bash
for gene in $(cat target_gene_ids.txt)
do
  echo $gene
  echo ">$gene"  
done
```

> Note that since the `>` character is special at the command line (it redirects output), we need to put what we want to give to `echo` 
> in quotes so that the whole thing goes to it (here “>$gene”).

Now let’s put the `grep` command we made above in the loop, and change what we’re looking for to be the `>` character followed by the `$` and variable name (just like it was provided to `echo`), and write the output to a new file:

```bash
for gene in $(cat target_gene_ids.txt)
do
  grep -w -A 1 ">$gene" genes.fa
done > target_genes.fa

ls
head target_genes.fa
```

And now we’ve made a new fasta file holding the sequences of just the genes we wanted!

## Looping until a condition is met

An alternative to looping over a predetermined number of items is to continue to loop until a condition is met. This is used less often, but is very important if you are chaining commands together and want to ensure the one step only starts after another step has completed.

```bash
while [ $previous_step_is_running ]
do
  sleep 1
done
echo $output
```

## If/then conditional
Loops are fantastic for scaling repetitive tasks across many files or lines. However, danger lies in not providing any check or balances for when things don't look right. Sometimes we want our code to behave differently if some critical information is missing. Enter if/else:

If/else statements at a Unix-like command-line at a basic level can look something like this:

```bash
if [ # something-to-be-evaluated # ]
then
    # do something if true #
else
    # do something else if false #
fi
```

The `fi` at the end is needed to close the if statement overall, similar to how we need to put `done` at the end of a for loop.

We can check if there is anything inside a file with `[ -s filename ]`. (I usually forget and need to google that whenever I have a situation where I want to do it). Let’s look at this altogether interactively on the command-line first. 

Here’s an example of how our if/then statement checking for file contents works (re-type, or copy and paste this code block into the command line):

```bash
if [ -s europa_target_gene_ids.txt ]
then
    echo "File is good-to-go!"
else
    echo "File is empty!"
fi
```

When we run that, the terminal prints back “File is empty!”:


If we run it on a file that holds something, like “target_gene_ids.txt” that we generated earlier, it prints back “File is good-to-go!”:

```bash
if [ -s target_gene_ids.txt ]
then
    echo "File is good-to-go!"
else
    echo "File is empty!"
fi
```


<blockquote>
<center><strong>QUICK QUESTION!</strong></center>

Now to try and combine for loops and if conditions. How might we write some code to check if the files within a folder are empty or not?

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">for file in folder/*
do
  if [ -s $file ]
  then
    echo "File is good-to-go!"
  else
    echo "File is empty!"
fi
done</code></pre>

</details>
</blockquote>

### Chaining if conditions

Unix shells have been around for decades and clever but lazy developers have built tonnes of shorthands to make complex logic code simpler. You really can spend years just learning new aspect of shells like bash. Here is an example of comparing an argument to multiple possible values:

```bash
if [[ "${my_var}" == "option_a" ]]
  then
    echo "I know what my_var is."
  elif [[ "${my_var}" == "option_b" ]]
  then
    echo "I still know what my_var is."
  else
    echo "I have no idea what my_var is."
fi
```

A really rabbit hole to get lost in with bash is its use of brackets. Normal, curl, squared, double or single all have different uses. The square brackets used above always resolve to a true or false value and have many inbuilt short handles for comparing different variables and files. Here we are just doing a simple string comparison. We are also using the elif shorthand for else if!

## Summary

Even though loops and logic can get much more complicated as needed, practicing these foundational skills a bit is all that’s needed to start harnessing their awesome power 🙂

#### Acknowledgements 

This training course was adapted from the [Happy Belly Bioinformatics Unix Course](https://astrobiomike.github.io/unix/).