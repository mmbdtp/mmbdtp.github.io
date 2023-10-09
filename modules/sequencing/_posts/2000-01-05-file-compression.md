---
title: File Compression and Gzip (`.gz`) 
---
File compression is the process of reducing the size of one or more files or folders to save disk space or reduce the time required for file transfer. Compression algorithms remove redundancy and encode data more efficiently.

In most Unix-like systems, `gzip` is pre-installed. To check if you have `gzip` installed, open a terminal and run:

```bash
gzip --version
```

If it's not installed, you can typically install it using your system's package manager (e.g., `apt`, `yum`, `brew`, etc.).

## Basic Usage of Gzip

### Compress a File

To compress a file using `gzip`, you can run the following command:

```bash
gzip filename
```

Replace `filename` with the name of the file you want to compress. This command will create a compressed file with the `.gz` extension, such as `filename.gz`.

### Decompress a File

To decompress a `.gz` file, use the `gunzip` command:

```bash
gunzip filename.gz
```

This will restore the original file, removing the `.gz` extension.

## Compression Options

- `-d` or `--decompress`: Decompress the specified file(s).
- `-c` or `--stdout`: Write compressed data to standard output (useful for piping data).
- `-k` or `--keep`: Keep the original file after compression (by default, the original file is removed).
- `-v` or `--verbose`: Display compression details.

## Examples

- To compress a file named `example.txt`:

  ```bash
  gzip example.txt
  ```

  This creates `example.txt.gz`.

- To decompress `example.txt.gz` and keep the original file:

  ```bash
  gunzip -k example.txt.gz
  ```

- To compress multiple files and display compression details:

  ```bash
  gzip -v file1.txt file2.txt file3.txt
  ```

## Compressing and Decompressing with Piping

You can also use `gzip` and `gunzip` with pipes to compress and decompress data on the fly. For example, to compress the output of a command and save it to a file:

```bash
command_to_generate_data | gzip -c > compressed_data.gz
```

And to decompress and process data from a compressed file:

```bash
gunzip -c compressed_data.gz | command_to_process_data
```
