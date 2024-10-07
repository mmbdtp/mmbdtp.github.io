---
title: Tuesday Afternoon — Using programs on Notebook servers 
---

## More fun with programs 

---
![](https://raw.githubusercontent.com/mmbdtp/mmbdtp.github.io/gh-pages/modules/UNIX/_posts/coli.webp)

### Quick analysis of an *E. coli* genome

Let's do a quick analysis of an *E. coli* genome with `seqkit`.

- **What is *Escherichia coli*?**
  - *E. coli* is a rod-shaped bacterium that can survive under a wide variety of conditions including variable temperatures, nutrient availability, and oxygen levels. Most strains are harmless, but some are associated with food-poisoning.

- **Why is *E. coli* important?**
  - *E. coli* is one of the most well-studied model organisms in science. As a single-celled organism, *E. coli* reproduces rapidly, typically doubling its population every 20 minutes, which means it can be manipulated easily in experiments. In addition, most naturally occurring strains of *E. coli* are harmless. Most importantly, the genetics of *E. coli* are fairly well understood and can be manipulated to study adaptation and evolution.

---

Let's download and analyse a real *E. coli* genome—that of the most commonly used lab strain MG1655!

```bash
$ wget "https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000005845.2/download?include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT&filename=GCF_000005845.2.zip" -O coligenome.zip
```

```bash
$ unzip coligenome.zip
```

```bash
$ seqkit stats -T -a -G -i ncbi_dataset/data/GCF_000005845.2/GCF_000005845.2_ASM584v2_genomic.fna > coli_genome_stats.tsv
```

---

Using the menu for the JupyterLab interface, select the `New Launcher` option. 

Now have a play with the interface's graphical user interface. Find and take a look at the files you have downloaded and unzipped and the .tsv file showing the results of the analysis. Download it on to your Mac and open it with Excel.

---

*Command Breakdown*

Here's a detailed explanation of what you just did.

---

`wget "https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000005845.2/download?include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT&filename=GCF_000005845.2.zip" -O coligenome.zip`

1. `wget`
   - This is a command-line utility used to **download files** from the internet. It is commonly used in Linux environments but is also available on other platforms.

2. `"https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000005845.2/download?include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT&filename=GCF_000005845.2.zip"`
   - This is the **URL** being passed to `wget`. It is a request to the NCBI Datasets API to download the genome data associated with the accession number **`GCF_000005845.2`**, which corresponds to a particular genome (in this case, *Escherichia coli str. K-12 substr. MG1655*).

   - The URL includes several important parameters:
     - `accession/GCF_000005845.2/download`: This is the NCBI identifier (GCF_000005845.2) for a specific genome assembly, and the command is asking for the download of this genome.
     - `include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT`**: This specifies the **types of annotations** to include in the download.
     
   - Specifically, it is asking for:
       - **GENOME_FASTA**: The genome sequence in FASTA format.
       - **GENOME_GFF**: Genome feature information in GFF (General Feature Format).
       - **RNA_FASTA**: RNA sequences in FASTA format.
       - **CDS_FASTA**: Coding sequences (CDS) in FASTA format.
       - **PROT_FASTA**: Protein sequences in FASTA format.
       - **SEQUENCE_REPORT**: A report about the sequence, usually in a structured or tabular format.
       - **`filename=GCF_000005845.2.zip`**: This specifies the **filename** for the ZIP file that will be downloaded from the NCBI. The ZIP file contains all the requested data.

3. `-O coligenome.zip`
   - The **`-O` option** in `wget` specifies the **output filename** for the downloaded file.
   - In this case, instead of saving the file with the default name (`GCF_000005845.2.zip`), the file will be saved locally as **`coligenome.zip`**.


This command uses `wget` to download a ZIP file from NCBI's Datasets API that contains various types of genomic data for the genome assembly **`GCF_000005845.2`** (which corresponds to *E. coli str. K-12 MG1655*, the most commonly used lab strain). The downloaded file will include the genome sequence in FASTA format, annotations in GFF format, RNA sequences, coding sequences, protein sequences, and a sequence report. The file is saved locally as `coligenome.zip`.


After running the command, you will have a ZIP file (`coligenome.zip`) that contains the genome and associated annotation files for *Escherichia coli str. K-12 substr. MG1655* from the NCBI Datasets API.

---
The command `unzip coligenome.zip` is used to extract the contents of a ZIP archive file named `coligenome.zip`. Here's a breakdown of the command:

- **`unzip`**: This is the command that extracts files from a ZIP archive. It's used to decompress files that have been compressed using the ZIP format.
  
- **`coligenome.zip`**: This is the name of the ZIP file that you want to extract. In this case, it's a file named `coligenome.zip`.

When you run the command, `unzip` will:
1. Open the `coligenome.zip` archive.
2. Extract its contents into the current directory.
3. List the files it has extracted.

If the ZIP file contains directories, `unzip` will also recreate the directory structure as it was in the original compressed archive.

If you want to extract the files into a specific directory, you can use the `-d` option like this:

```
unzip coligenome.zip -d /path/to/destination
```

This would extract the contents of `coligenome.zip` into the specified directory.

---

`seqkit stats -T -a -G -i ncbi_dataset/data/GCF_000005845.2/GCF_000005845.2_ASM584v2_genomic.fna > coli_genome_stats.tsv`

This command is using the **SeqKit** tool to generate statistics about a genome file in FASTA format, and then save the results to a file named `coli_genome_stats.tsv`.


1. `seqkit stats`
   - This is the **subcommand** of the SeqKit tool used to calculate various statistics about sequences in a FASTA or FASTQ file. It provides information such as sequence length, GC content, number of sequences, and other useful metrics for analyzing genomic data.

2. `-T`
   - This flag tells SeqKit to output the statistics in a **tabular format**, using tab-delimited columns. It makes the output easier to import into spreadsheet software or to process further in other tools.

3. `-a`
   - The `-a` flag tells SeqKit to calculate and display **additional statistics**, including:
     - N50 (the sequence length at which 50% of the genome is covered by contigs of that length or longer).
     - N90 (same concept as N50, but with 90% coverage).
     - Total sequence length, etc.
   These additional metrics are useful for assessing genome assembly quality.

4. `-G`
   - This flag **includes the GC content** in the statistics. GC content is a commonly analyzed metric that refers to the percentage of nucleotides in the sequences that are either guanine (G) or cytosine (C). The GC content can give insights into the composition and properties of the genome.

5. `-i`
   - This flag **includes file information** (such as file name and file type) in the output. This can be helpful for keeping track of which file the statistics were calculated from, especially when handling multiple datasets.

6. `ncbi_dataset/data/GCF_000005845.2/GCF_000005845.2_ASM584v2_genomic.fna`
   - This is the **input file**. It is the path to the FASTA file containing the genomic sequences for which you want to generate statistics.
   - This specific file is a genomic FASTA file for *Escherichia coli* strain K-12 (genome assembly ID: **GCF_000005845.2**).

7. `> coli_genome_stats.tsv`
   - The **`>` operator** is used to **redirect the output** of the command to a file.
   - In this case, the statistics calculated by SeqKit will be written to a file named **`coli_genome_stats.tsv`** instead of being displayed on the screen (standard output).
   - **`coli_genome_stats.tsv`** will be a tab-separated file (due to the `-T` flag) containing the genome statistics, which can be easily opened in a text editor or spreadsheet software for analysis.


This command calculates detailed statistics on the genomic FASTA file **`GCF_000005845.2_ASM584v2_genomic.fna`**, including sequence length, N50/N90 values, GC content, and file metadata. The output is saved in **tabular format** to a file named `coli_genome_stats.tsv`, which can be easily read and analyzed later.


The output will be a TSV (tab-separated values) file named `coli_genome_stats.tsv` with columns representing the following statistics:

- Sequence ID
- Sequence count
- Total sequence length
- N50, N90, and other assembly metrics
- GC content
- File information

You can open `coli_genome_stats.tsv` in any text editor, spreadsheet program (like Excel), or data analysis software for further exploration.

What do you think these statistics are telling us? Why are they important? Don't worry of it all looks confusing. We will be covering sequence file formats and QC later.

## More uses of SeqKit

Here are some more examples of using `seqkit` on thr *E. coli* DNA or protain sequences. 

Look at the [documentation](https://bioinf.shenwei.me/seqkit/usage/) and the outputs you get and see if you can work out what each command is doing.

```
seqkit seq -m 1000 ncbi_dataset/data/GCF_000005845.2/protein.faa
```

```
seqkit head -n 100 ncbi_dataset/data/GCF_000005845.2/protein.faa
```

```
seqkit stats ncbi_dataset/data/GCF_000005845.2/protein.faa
```

```
seqkit grep -p "NP_414651.1" ncbi_dataset/data/GCF_000005845.2/protein.faa
```

```
seqkit sort -l -r ncbi_dataset/data/GCF_000005845.2/protein.faa
```

```
seqkit subseq -r 1000:2000 ncbi_dataset/data/GCF_000005845.2/GCF_000005845.2_ASM584v2_genomic.fna
```

---

**Coffee break**

---

## Let's play with another program, SeqFu

**SeqFu** is a fast and lightweight bioinformatics toolkit designed for working with sequencing data, particularly in FASTA and FASTQ formats. It provides a variety of utilities for tasks such as sequence manipulation, quality control, and statistics calculation. SeqFu is known for its simplicity and efficiency, making it popular for handling large datasets in genomics pipelines. It can perform tasks such as splitting sequences, filtering reads, trimming, and calculating sequence statistics.

 - [https://telatin.github.io/seqfu2/](https://telatin.github.io/seqfu2/)
 - [https://www.mdpi.com/2306-5354/8/5/59](https://www.mdpi.com/2306-5354/8/5/59)

SeqFu was developed by [Andrea Telatin](https://quadram.ac.uk/people/andrea-telatin/), head of bioinformatics at the Quadram Institute.

---

**Installing SeqFu**

First we are going to have to install SeqFu. Remember that you cannot do this in your base Conda environment, so take care to ensure you are already in the `bioinfo_fun` environment or activate it if you aren't!

Now install SeqFu! See if you can work out how to do that for yourselves using Conda!

---

Let's check you have SeqFu installed:

```
seqfu version
```

Now we can use the `seqfu` tool to count the number of protein sequences associated with the *E. coli* genome:

    
```
 seqfu count ncbi_dataset/data/GCF_000005845.2/protein.faa
    
```
    

And we can collect more statistics with `seqfu stats`:

    
```
seqfu stats -nt ncbi_dataset/data/GCF_000005845.2/protein.faa
    
```
    
Where:

*   `-n` will print a “nice” output, with a terminal table (default is TSV)
*   `-t` will add the thousands separator (default is off)

the output should be:

    
```    
    
┌───────────────────────────────────────────────┬───────┬───────────┬────────┬─────┬─────┬─────┬────────┬─────┬───────┐
│ File                                          │ #Seq  │ Total bp  │ Avg    │ N50 │ N75 │ N90 │ auN    │ Min │ Max   │
├───────────────────────────────────────────────┼───────┼───────────┼────────┼─────┼─────┼─────┼────────┼─────┼───────┤
│ ncbi_dataset/data/GCF_000005845.2/protein.faa │ 4,298 │ 1,329,980 │ 309.44 │ 393 │ 271 │ 180 │ 452.98 │ 8   │ 2,358 │
└───────────────────────────────────────────────┴───────┴───────────┴────────┴─────┴─────┴─────┴────────┴─────┴───────┘
```

**SeqFu** has a long list of tools, some are modeled after the Unix tools we have seen so far, but with a more powerful syntax and more options.

Let's work through some examples looking at protein and DNA sequences.

`seqfu head` prints the first _n_ sequences (also taking 1 every _y_)
```
seqfu head ncbi_dataset/data/GCF_000005845.2/protein.faa
```

`seqfu tail` will print the last _n_ sequences
```
seqfu tail ncbi_dataset/data/GCF_000005845.2/protein.faa
```

`seqfu bases` counts the number of A, C, G, T and Ns in a DNA file.

```seqfu bases -n ncbi_dataset/data/GCF_000005845.2/GCF_000005845.2_ASM584v2_genomic.fna

```

Take a look at the documentation and play with the options: [[https://telatin.github.io/seqfu2/]](https://telatin.github.io/seqfu2/).

If you have some time to spare, try installing other software from the [Bioconda repository](https://bioconda.github.io/conda-package_index.html)

---


That's it for today. See you on Thursday!
