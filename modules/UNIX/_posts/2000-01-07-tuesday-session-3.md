---
title: Tuesday - Session 3
---

# An introduction to PATH, startup files and aliases

> **Things covered here:**
> 
> - What is PATH?
> - startup files
> - aliases

## The PATH demystified
As mentioned earlier in the course, one of the easiest mistakes to make at the command line is to be trying to specify a file or program that isn’t where we think it is. For files, we usually point to where the file is using an *absolute* or *relative* path, where “path” here, in lowercase, just means a sort of address in the computer. But for programs that we use often, we usually want to be able to call them without having to provide the path to wherever the program is located. So a big part of getting specific programs to work properly at the command line is having them in a location on the computer that we can access no matter where we are.

The command line automatically checks in a list of pre-defined locations (directories) everytime we are trying to call a certain command. This is why we can use certain commands like `ls` and `pwd` and such from any directory we happen to be in. This list of pre-designated directories is stored in a special variable called “PATH” (all caps required). We can see our PATH, and which directories are stored in it, by entering `echo $PATH` at the command line (the `$` is used to call variables in bash and other Unix languages as we discussed before). Here’s a look at a rather messy one of mine:


This is a colon-delimited list of all the directories the command line looks in by default for programs on the particular computer I’m on right now. To make it a little friendlier to glance at, we can change the colons to newline characters by piping the output of `echo $PATH` into `tr`, to change the colons to newline characters for a more user-friendly output:

```bash
echo $PATH | tr ":" "\n"
```

We can now more clearly see this is a list of directories. All of these places, stored in the variable called “PATH”, are searched whenever we are typing a command in the terminal window. If the command we are trying to use is present in any of the directories listed in our PATH, we don’t need to point at its specific location in full (its path, lowercase) when we are trying to use it – which is of course nice for things we use often.

**To make a program available anywhere, we can either place that program in a directory that’s already in our PATH, or we can add a new directory to our PATH that contains the program.** (Keep in mind that the order in which things appear in our PATH *does* matter. If we have two versions of a program with the same name, whichever shows up first will be the one that’s used.)


## Adding a directory to our PATH

To demonstrate how to add a directory to our PATH, we’re going to create a new directory and within it make a quick bash script that tells us what time it is. We’re then going to add that directory to our PATH so that we can use the time-telling script from anywhere. If you want to follow along, you can make both by copying and pasting the following code block.

```bash
mkdir my-bin
cd my-bin

cat >> what-time-is-it.sh << 'EOF'
#!/bin/bash

current_time=$(date | tr -s " " "\t" | cut -f 4 | cut -d ":" -f 1,2)

echo "The time is $current_time.
I'm glad to see you're making good use of it :)"

EOF

chmod +x what-time-is-it.sh

ls
cat what-time-is-it.sh
```

Ok great, so we just wrote a program that tells us what time it is according to our computer. That `cat >> what-time-is-it.sh << 'EOF'` line is say to put whatever we type that follows into that file we are just creating, up until we type “EOF” for end of file. Then for the little example script: `#!/bin/bash` tells the computer which program to use when executing the script; the `current_time` line is us setting a variable, called “current_time”, and storing within it the time that we use some unix magic to cut out of what the command date outputs; and then we are having it `echo` out the sentences (print to the terminal) and inserting the variable `$current_time`. Note the `$` here is just calling the variable in the same way as when we did echo `$PATH` above. The last little part `chmod +x what-time-is-it.sh` is changing the properties of the file so that the computer knows it’s a program and will let us execute it more conveniently.

Let’s give it shot now. At the moment, the “what-time-is-it.sh” script is *not* in our PATH. It exists only in the directory we are sitting in, and that directory is not in the list of directories that pops up when we run `echo $PATH`. So right now, to execute the program, we need to tell the computer where it is with its relative or absolute path. (*Executing* a file is different than *doing something to it* like we did with the cat command). And to *execute* a program, we need to be a bit more explicit even if it’s sitting in our current working directory. Here we’ll use the relative path, which looks like this:

```bash
./what-time-is-it.sh
```

And note, if we are not in this working directory that contains the script, we have to point to it in much the same way. Here, let’s move up one level and try again:

```bash
cd ..

./what-time-is-it.sh 

my-bin/what-time-is-it.sh 
```

Now to make our program accessible to us wherever we are, without having to point at its location (its path, lowercase), we’re going to add its directory, `my-bin`, to the list of pre-specified directories in our PATH (the variable, all caps). To remove as much of our mortal enemy as possible from the process (human error), it’s easiest to just change into the directory we want to add, run `pwd`, and copy the absolute path:

```bash
cd my-bin/
pwd
```

Now that the absolute path of the directory is patiently waiting in the purgatory between copy and paste, we can modify our PATH to include it. This can be done temporarily or permanently, like we covered before. Let's run through it quickly againin this context:

### Temporarily

Running the following code modifies the PATH just for the current terminal session, so when we close the window it will be gone:

```bash
export PATH="$PATH:/Users/<username>/my-bin"
```

Here, `export` is the command we are using, then we are specifying the variable we want to set, “PATH”. Then we are saying we want to set the “PATH” variable to include everything that is already in the PATH, by first putting `$PATH`, then we put a colon, which we saw above is what delimits the list of directories in the PATH variable, and then we added our new directory, `/Users/<username>/my-bin` – `<username>` is the part you would change to match yours. (The `export` part of this is a little more into the weeds than we need here, but basically it sets the variable for any sub-processes launched by the terminal.) Now if we look at my PATH like we did above, we see that at the end the directory `/Users/<username>/my-bin` is included!

```bash
echo $PATH | tr ":" "\n"
```

Beautiful, and to see the benefits, we can now run our `what-time-is-it.sh` program without pointing to its location. Here is in the directory it sits (without needing to have the `./` like we needed above):

```bash
cd ~/my-bin
what-time-is-it.sh
```

But we can also be anywhere else now:

```bash
cd ..
what-time-is-it.sh
```

### Permanently

As noted above, that method only temporarily modifies our PATH, which is sometimes useful. But often we will want to modify it permanently. To do so we need to edit a sort of special file, there are a few of these, but that is a concept for another page. The one we are going to use here is called `~/.bash_profile` (files with a `.` in front of them are “hidden” files). This file either already exists in our home directory or we will create it if it doesn’t yet, and it gets run everytime we open a terminal window. This file and the others like it are what allow us to customize our terminal window with things like setting variables we always want, adding color schemes, or modifying our prompt. Here is one way we can permanently add a directory to our PATH by using `echo` to append the code to the end of that file:

> **Note:**
> If we are working on a server or cluster, it may be the case that we actually want to modify our 
> PATH varible in the `~/.profile` file instead of the `~/.bash_profile` file as done below. If you check your 
> home location with `ls -a ~/` and you have a `~/.profile` but no `~/.bash_profile`, then change the two example 
> lines below so that `~/.profile` is used in place of `~/.bash_profile`.

<br>

```bash 
echo 'export PATH="$PATH:/Users/<username>/my-bin"' >> ~/.bash_profile
```

Note that the code is exactly the same as we ran above, but now we’re appending it to the `~/.bash_profile`. And since this file gets run each time we open a terminal window, it’s the same thing as if we did it ourselves everytime we opened a terminal window – except much better of course because we don’t have to actually do it ourselves. Keep in mind that doing it this way, where we `echo` the text needed into the file, isn’t the only way to do this. The `~/.bash_profile` is just a text file, so we could open it with a regular text editor or a terminal-based one like nano and enter the same text that way. Also, since this file is run everytime we open a terminal session, it actually hasn’t been run yet since we just updated it right now, so our PATH variable hasn’t yet been updated to include the directory we just added. So we can either open a new terminal session, or we can run the `source` command on the `~/.bash_profile` file like this:

```bash
source ~/.bash_profile
```

And that’s it! The `PATH` variable is just a special variable that contains all of the directories that are automatically searched when we try to call a program. Feel free to delete the `what-time-is-it.sh` script, but consider keeping the `my-bin` directory as a place to put things if you want them to be available from anywhere. Now that this directory is already in your PATH, you won’t have to worry about that part anymore and anything you put in there will be accessible from anywhere on that computer.

### One last important note

We can add any directories to our PATH that we’d like, **but we must be sure to always include the `$PATH` variable like that in the list as we edit it**, otherwise we might get stuck with no regular commands working anymore (like `ls`, `pwd`, `wc`, etc.). If that happens, don’t despair! We can open that `~/.bash_profile` in any regular text editor (we may have to select “show hidden files” or something like that in the Finder window in order to see it), and then just delete whatever was added that messed things up. Then we’ll be able to launch a new terminal again that works just fine and try again!


## What is a startup file?

A large part of the value of working at a Unix-like command-line is the way it easily lets us automate things. In this vein, there are many things we can do to customize our Unix-like command-line environment, including things to make ourselves more efficient when working there.

High-level, a startup file is a file that is automatically run everytime a command-line session is started. startup files are integral to setting up our working environment properly. These files are just like shell scripts, in that they do the same thing as if we were running each line one at a time by ourselves, but it is all taken care of for us automatically whenever a new session is started.

As mentioned, these files are integral to setting up our working environment, and while unlikely, it is possible we could mess them up. Don’t worry about that too much though. There is a standard template for all users, and if things somehow went wrong, we could have whoever handles our user account just replace the one we messed up if needed. And we can always make a backup copy of them before starting to alter them if wanted 🙂

> **Note:**
> The actual filenames of the startup files used below are appropriate if we are using a bash shell, making the filenames, e.g., `~/`.
> bashrc and `~/.bash_aliases`. If working with a different shell, these files will be named differently – e.g., if using `Z shell`, it
> would be `~/.zshrc` and` ~/.zsh_aliases`, and the files in codeblocks below would need to be changed accordingly. We can see which 
> shell we are using by running `echo $0`.


## What is an alias?

An alias is typically just a shorthand for a longer one-liner command. For instance, here is a contrived example of how we can set an alias for the current session.

`date` by itself prints out a few things, but we can get just the day with `date +%A`:

If this were something we used a lot, we could set an alias to execute `date +%A` for us without us needing to type out the full command. Here we are making an alias called `today` to do this for us.

If we just try to run `today` right now, we will get a command not found error. Here’s how we set it:

```bash
alias today="date +%A"
```

And now we’ll get the day:


But doing things like above, it is only for the current session, and it will be gone when we exit and sign back in. To keep an alias, we need it to be set by one of the startup scripts, which we’ll cover now.

### Where to set a permanent alias

In the `~/.bashrc` file again, there are likely also some lines that look like this (if they don’t exist, you can add them):

```bash
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

As mentioned above, this `~/.bashrc` file is run everytime a session is started. In that file, these lines of code are saying “if the file `~/.bash_aliases` exists, then also run that file to setup what is in there”. (We could just put aliases in the `~/.bashrc`, but it’s also common practice to keep them in their own file as we’ll do here.)

So we are going to add our commands that set up our aliases to that `~/.bash_aliases` file, and then they will be loaded anytime we start a new session – therefore making them always available to us.

### How to set a permanent alias

Just like with modifying the `~/.bashrc` file, it’s generally easiest to add these with a text editor like `nano` (which is what I’ll be using here).

Here’s how we would add the above, just to serve as an example of how we can add them permanently. We can edit (and/or create if it doesn’t yet exist) the `~/.bash_aliases` file with the following:

```bash
nano ~/.bash_aliases
```

And then paste in our command to create the alias:

```bash
alias today="date +%A"
```

To save and exit nano, we can press `ctrl+x`, followed by the key `y`, followed by pressing `enter/return`.

And remember we need to source the `~/.bashrc` file for the changes to take effect in our current session:

```bash
source ~/.bashrc
```

Now that alias will always be there when we sign in. But we probably don’t want to keep that one, so we could always delete that line from the `~/.bash_aliases` file with any editor (like `nano`). This was just an example to explain the process, but now we’ll look at more useful ones.


### Some useful aliases

Below are some of the aliases I typically setup on any machine I will be working on regularly.

#### Store last command

I like to create an alias that appends the last command I ran into a file called ‘log’ in the current working directory. I find this really handy when I am testing/figuring things out.

When I find the command I want, and I want to save it, instead of copying and pasting the last thing I ran, I can just type `store`, and it is added to the log file for me.

This one-liner will do this for us:

```bash
history -p !!:p >> log; printf "\n" >> log
```

Which would be annoying to type out in full everytime we want to use it, so instead, let’s create an alias in our `~/.bash_aliase`s file by coping and pasting this into that file (see example just above if needing a reminder on using `nano` to do this):

```bash
alias store='history -p !!:p >> log; printf "\n" >> log'
```

And remember we need to run this so the changes to be in effect in our current session:

```bash
source ~/.bashrc
```

Now whenever we want, we can run store to save the last command we ran in a file called ‘log’ in the current working directory:

#### Get the size of and sort all items in a directory

When in the crunch of trying to see where all our storage has disappeared to, I find it convenient to run something like this which will :

```bash
du -sh * | sort -h
```

But I don’t like to remember or type all that out. So I add this alias to my `~/.bash_aliases` file:

```bash
alias dush='du -sh * | sort -h'
```

```bash
source ~/.bashrc
```
Now just running `dush` will list the sizes of all directories and files in the current working directory in order of size.


#### Printing in formatted columns

`column` is a handy command for quickly viewing plain text tables like tab-separated value (tsv) and comma-separated value (csv) files, but I typically like to give it a few arguments I’d rather not type out each time. So I have these for tsv and csv in my `~/.bash_aliases` file:

```bash
alias col-t="column -ts $'\t'"

alias col-c="column -ts ','"
```

```bash
source ~/.bashrc
```

So it’s easier to quickly check out table files in a more organized fashion (note I typically pipe the output of `head` or `tail` into this):


#### SSH connections

It's common to connect to remote computers pretty much all day everyday. So we don’t always want to type out the connection (especially for ones where we need to use an IP address intead of words). So I will typically add an alias for connecting to any remote machine I use regularly. E.g., if this were real, I would add this to my `~/.bash_aliases` file:

```bash
alias hpc="ssh <username>@hpc.nbi.ac.uk"
```

```bash
source ~/.bashrc
```

And now we would just have to run hpc to connect and get my password prompt.


#### Acknowledgements

This training course was adapted from the [Happy Belly Bioinformatics Unix Course](https://astrobiomike.github.io/unix/).