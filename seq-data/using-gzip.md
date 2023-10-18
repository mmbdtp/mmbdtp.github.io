---
layout: page
---

# Using tar and gzip for fun and profit

> Gzip (GNU zip) compression is a widely used file compression and decompression format and tool that reduces the size of files or data streams to save storage space or reduce data transfer times over a network. It was developed as part of the GNU project and is commonly found in Unix-like operating systems, including Linux.

> tar is a command-line utility and a file format used for archiving and compressing files and directories. The term "tar" stands for "tape archive," refering to when data was stored on magnetic tapes. The "tar" utility is a commonly used tool for creating, extracting, and managing archive files in the tar format.

## Example files 
Make some example files for us to play with.

You can use any combination of these, or make your own. Make at least 3 files. You can use a terminal text editor like vi or emacs, but do not use the GUI text editor.

```bash
grep --help   > grep_help.txt
touch empty_file.txt
echo "This is some text" > sometext.txt
```

### Exercise 1: Creating files

**Create the some example file, as described above.** 

**What is `touch`? What does `touch` do if the file already exists?** 

**From the example above, What does the `>` do for the grep output?**

**What does `echo` do?**

## Using gzip
The remainered of this module will use the example files you've created to demonstrate how to use `gzip` and `tar`. Gzip allows use to compress files, while tar allows us to group files together. Let's start with gzip. 

`gzip` should be pre-installed. To check if you have `gzip` installed, open a terminal and run:

```bash
gzip --version
```
If it's not installed, (tell us!) but you can typically install it using your system's package manager (e.g., `apt`, `yum`, `brew`, etc.).

#### Compress a File

To compress a file using `gzip`, you can run the following command:

```bash
gzip filename
```

Replace `filename` with the name of the file you want to compress. This command will create a compressed file with the `.gz` extension, such as `filename.gz`.

#### Decompress a File

To decompress a `.gz` file, use the `gunzip` command:

```bash
gunzip filename.gz
```

This will restore the original file, removing the `.gz` extension.

#### Compression Options

- `-d` or `--decompress`: Decompress the specified file(s).
- `-c` or `--stdout`: Write compressed data to standard output (useful for piping data).
- `-k` or `--keep`: Keep the original file after compression (by default, the original file is removed).
- `-v` or `--verbose`: Display compression details.

#### Compressing and Decompressing with Piping

You can also use `gzip` and `gunzip` with pipes to compress and decompress data on the fly. For example, to compress the output of a command and save it to a file:

```bash
command_to_generate_data | gzip -c > compressed_data.gz
```

And to decompress and process data from a compressed file:

```bash
gunzip -c compressed_data.gz | command_to_process_data
```

### Exercise 2: Creating files

**With the information above, and whatever else you can find via the internet, compress the files you created in Exercise 1.**

**Now reverse it by decompressing those files**

**What is the difference in size of the compressed files and the original files?**

**Can you compress one of the files while keeping the original (decompressed)?** 

**What happens to the file extension after compressing with gzip?**

**Can you combine the steps in exercise 1 and 2, to create and compress a file in one step?**

This is difficult. Hint, use piping. 


[Next: Sequence file formats]({{site.baseurl}}/modules/sequencing/sequence-data/).

[Back to Programme]({{site.baseurl}}/modules/sequencing/week-2-programme/).
