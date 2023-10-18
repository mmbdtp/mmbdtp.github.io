---
layout: page
---

# Exercise answers for: Using tar and gzip for fun and profit

[Back to gzip exercise](/seq-data/using-gzip)

### Exercise 1: Creating files

**Create the some example file, as described above.** 

```bash
grep --help   > grep_help.txt
touch empty_file.txt
echo "This is some text" > sometext.txt
```

**What is `touch`? What does `touch` do if the file already exists?** 

Update the access and modification times of each FILE to the current time.
A FILE argument that does not exist is created empty, unless `-c` or `-h` is supplied.

see `man touch` for more information.

**From the example above, What does the `>` do for the grep output?**

The `>` redirects the standard output of the command to the specified file. `>` will overwrite the location, if the file already exists. `>>` Will append. 

**What does `echo` do?**

Echo the STRING(s) to standard output.

see `man echo` for more information.

**How can you view the contents of these files?**

`cat` is one way to view the contents of a file. `cat` will print the contents of a file to standard output. Try other options like `head`, `less`, `more`, `tail`, `nl`. You can also use a text editor like `vi` or `emacs`.

### Exercise 2: Using `gzip`

**With the information above, and whatever else you can find via the internet, compress the files you created in Exercise 1.**

```bash
gzip grep_help.txt
```

**Now reverse it by decompressing those files**

```bash
gunzip grep_help.txt.gz
```

OR 

```bash
gzip -d grep_help.txt.gz
```

**What is the difference in size of the compressed files and the original files?**

* grep_help.txt:  4042 bytes
* grep_help.txt.gz:  1580 bytes

**Can you compress one of the files while keeping the original (decompressed)?** 

```bash
gzip -k grep_help.txt
```

**What happens to the file extension after compressing with `gzip`?**

The file extension changes from `.txt` to `.txt.gz`. i.e. gz is appended to the end. 

**How can you view the contents of these files?**

```
zcat grep_help.txt.gz
```

**What is the difference between `|` and `>`?**

`|` is a pipe. It redirects the standard output of the command on the left to the standard input of the command on the right. 

`>` redirects the standard output of the command on the left to the specified file. `>` will overwrite the location, if the file already exists. `>>` Will append.

**Can you combine the steps in exercise 1 and 2, to create and compress a file in one step?**

This is difficult. Hint, use piping. 

```bash 
grep --help | gzip -c > grep_help.txt.gz
```

Did you notice that we needed to specify the `-c` flag for `gzip`? This is because `gzip` expects a file as input, not standard input. The `-c` flag tells `gzip` to read from standard input. Did you also notice that we needed to give the full file extension for the output file? This is because `gzip` will not automatically append the `.gz` extension when reading from standard input. Ask Nabil why. 

[Back to gzip exercise](/seq-data/using-gzip)

## Using tar

Don't forget to ask Arnie:

![Alt text](image-1.png)


### Exercise 3: Using `tar`

**With the information above, and whatever else you can find via the internet, bundle all the files you created in Exercise 1 into a tarball**

```bash
tar -cvf my_files.tar grep_help.txt sometext.txt other_file.txt
```

**Now reverse it by extracting those files**

**What is the difference in size of the `tar` file and the sum of the original files?**

**What is the difference in file name of the output file between directly compressing with `gzip` and using `tar`?**

**What is the purpose of the `-v` flag in the `tar` command.**

**Can you combine the steps in exercise 2 and 3, to create an archive and compress it in one step?**

i.e. create a `.tar.gz` file in one step. 



[Back to gzip exercise](/seq-data/using-gzip)

