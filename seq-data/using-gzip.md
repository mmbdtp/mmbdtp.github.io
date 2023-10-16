---
layout: page
---

# Using tar and gzip for fun and profit

Gzip (GNU zip) compression is a widely used file compression and decompression format and tool that reduces the size of files or data streams to save storage space or reduce data transfer times over a network. It was developed as part of the GNU project and is commonly found in Unix-like operating systems, including Linux.

Here's how gzip compression works:

1. **Data Compression**: Gzip uses the DEFLATE algorithm, which is a combination of LZ77 (Lempel-Ziv 77) and Huffman coding. This algorithm analyzes the input data for repeated sequences and replaces them with shorter representations. It is lossless, meaning that the original data can be fully reconstructed upon decompression.

2. **File Format**: When gzip compresses a file, it creates a new file with a ".gz" extension. For example, if you have a file named "example.txt," running gzip on it will produce a compressed file called "example.txt.gz." The compressed file retains the original file's metadata, including permissions and timestamps.

3. **Compression Ratio**: The compression ratio achieved by gzip depends on the nature of the data being compressed. Text files, log files, and other human-readable content often achieve good compression ratios, sometimes reducing the file size to a fraction of the original. In contrast, already compressed files like media files (e.g., MP3, JPEG) may not compress significantly.

4. **Decompression**: To decompress a gzip-compressed file, you can use the `gzip` command-line tool, or various software applications that support gzip decompression. The command to decompress a gzip file is simply `gzip -d filename.gz` or `gunzip filename.gz`.

5. **Streaming**: One of the advantages of gzip is that it supports streaming. You can compress or decompress data on the fly without the need to create intermediate files. This is especially useful for tasks like compressing log files as they are generated.

Here's an example of how to use gzip for compression:

```bash
# To compress a file
gzip filename

# To decompress a file
gzip -d filename.gz
```

Gzip is a fast and efficient compression method and is commonly used in web servers to compress content before sending it to clients. Modern web browsers support gzip compression, which allows websites to load faster by transmitting less data over the internet.


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

[Back to Programme]({{site.baseurl}}/modules/sequencing/week-2-programme/).
