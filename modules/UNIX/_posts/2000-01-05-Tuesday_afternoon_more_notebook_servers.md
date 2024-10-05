---

title: Tuesday Afternoon— Using programs on Notebook servers 


---

## More fun with programs 



### Quick analysis of an *E. coli* genome

Let's do a quick analysis of an *E. coli* genome with `seqkit`.

- **What is *E. coli*?**
  - *E. coli* is a rod-shaped bacterium that can survive under a wide variety of conditions including variable temperatures, nutrient availability, and oxygen levels. Most strains are harmless, but some are associated with food-poisoning.

[](https://species.wikimedia.org/wiki/Escherichia_coli#/media/File:EscherichiaColi_NIAID.jpg)

- **Why is *E. coli* important?**
  - *E. coli* is one of the most well-studied model organisms in science. As a single-celled organism, *E. coli* reproduces rapidly, typically doubling its population every 20 minutes, which means it can be manipulated easily in experiments. In addition, most naturally occurring strains of *E. coli* are harmless. Most importantly, the genetics of *E. coli* are fairly well understood and can be manipulated to study adaptation and evolution.


Let's download and analyse a real *E. coli* genome—that of the most commonly used lab strain MG1655!

```
wget "https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000005845.2/download?include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT&filename=GCF_000005845.2.zip" -O coligenome.zip
unzip coligenome.zip

seqkit stats -T -a -G -i ncbi_dataset/data/GCF_000005845.2/GCF_000005845.2_ASM584v2_genomic.fna > coli_genome_stats.tsv

```

Using the menu for the JupyterLab interface, select the `New Launcher` option. 

Now have a play with the interface's graphical user interface. Find and take a look at the files you have downloaded and unzipped and the .tsv file showing the results of the analysis. Download it on to your Mac and open it with Excel.


::::::::::::::::::::::::::::::::::::::: Command Breakdown
Here's a detailed explanation of what you just did.

```
wget "https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000005845.2/download?include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT&filename=GCF_000005845.2.zip" -O coligenome.zip
```

1. **`wget`**:
   - This is a command-line utility used to **download files** from the internet. It is commonly used in Linux environments but is also available on other platforms.

2. **`"https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000005845.2/download?include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT&filename=GCF_000005845.2.zip"`**:
   - This is the **URL** being passed to `wget`. It is a request to the NCBI Datasets API to download the genome data associated with the accession number **`GCF_000005845.2`**, which corresponds to a particular genome (in this case, *Escherichia coli str. K-12 substr. MG1655*).

   - The URL includes several important parameters:
     - **`accession/GCF_000005845.2/download`**: This is the NCBI identifier (GCF_000005845.2) for a specific genome assembly, and the command is asking for the download of this genome.
     - **`include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT`**: This specifies the **types of annotations** to include in the download. Specifically, it is asking for:
       - **GENOME_FASTA**: The genome sequence in FASTA format.
       - **GENOME_GFF**: Genome feature information in GFF (General Feature Format).
       - **RNA_FASTA**: RNA sequences in FASTA format.
       - **CDS_FASTA**: Coding sequences (CDS) in FASTA format.
       - **PROT_FASTA**: Protein sequences in FASTA format.
       - **SEQUENCE_REPORT**: A report about the sequence, usually in a structured or tabular format.
     - **`filename=GCF_000005845.2.zip`**: This specifies the **filename** for the ZIP file that will be downloaded from the NCBI. The ZIP file contains all the requested data.

3. **`-O coligenome.zip`**:
   - The **`-O` option** in `wget` specifies the **output filename** for the downloaded file.
   - In this case, instead of saving the file with the default name (`GCF_000005845.2.zip`), the file will be saved locally as **`coligenome.zip`**.


This command uses `wget` to download a ZIP file from NCBI's Datasets API that contains various types of genomic data for the genome assembly **`GCF_000005845.2`** (which corresponds to *E. coli str. K-12 MG1655*, the most commonly used lab strain). The downloaded file will include the genome sequence in FASTA format, annotations in GFF format, RNA sequences, coding sequences, protein sequences, and a sequence report. The file is saved locally as `coligenome.zip`.


After running the command, you will have a ZIP file (`coligenome.zip`) that contains the genome and associated annotation files for *Escherichia coli str. K-12 substr. MG1655* from the NCBI Datasets API.


`seqkit stats -T -a -G -i ncbi_dataset/data/GCF_000005845.2/GCF_000005845.2_ASM584v2_genomic.fna > coli_genome_stats.tsv`

This command is using the **SeqKit** tool to generate statistics about a genome file in FASTA format, and then save the results to a file named `coli_genome_stats.tsv`.


1. **`seqkit stats`**:
   - This is the **subcommand** of the SeqKit tool used to calculate various statistics about sequences in a FASTA or FASTQ file. It provides information such as sequence length, GC content, number of sequences, and other useful metrics for analyzing genomic data.

2. **`-T`**:
   - This flag tells SeqKit to output the statistics in a **tabular format**, using tab-delimited columns. It makes the output easier to import into spreadsheet software or to process further in other tools.

3. **`-a`**:
   - The `-a` flag tells SeqKit to calculate and display **additional statistics**, including:
     - N50 (the sequence length at which 50% of the genome is covered by contigs of that length or longer).
     - N90 (same concept as N50, but with 90% coverage).
     - Total sequence length, etc.
   These additional metrics are useful for assessing genome assembly quality.

4. **`-G`**:
   - This flag **includes the GC content** in the statistics. GC content is a commonly analyzed metric that refers to the percentage of nucleotides in the sequences that are either guanine (G) or cytosine (C). The GC content can give insights into the composition and properties of the genome.

5. **`-i`**:
   - This flag **includes file information** (such as file name and file type) in the output. This can be helpful for keeping track of which file the statistics were calculated from, especially when handling multiple datasets.

6. **`ncbi_dataset/data/GCF_000005845.2/GCF_000005845.2_ASM584v2_genomic.fna`**:
   - This is the **input file**. It is the path to the FASTA file containing the genomic sequences for which you want to generate statistics.
   - This specific file is likely a genomic FASTA file for *Escherichia coli* strain K-12 (genome assembly ID: **GCF_000005845.2**).

7. **`> coli_genome_stats.tsv`**:
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


::::::::::::::::::::::::::::::::::::::::::::::::::

Using SeqFu
-----------------------------

Using [mamba (or conda)](/microbiome-bioinformatics/Install-Miniconda/) we can install the [`seqfu`](https://telatin.github.io/seqfu2/) tool:

    
```
# We request a version above 1.9 (at the time of writing, the last version is 1.16)
mamba install -c bioconda -c conda-forge "seqfu>1.9"
    
```
    

Now we can use the `seqfu` tool to count the number of sequences:

    
```
 seqfu count vir_protein.faa.gz
    
```
    

And we can collect more statistics with `seqfu stats`:

    
```
seqfu stats -nt vir_protein.faa.gz dummy.fa
    
```
    
Where:

*   `-n` will print a “nice” output, with a terminal table (default is TSV)
*   `-t` will add the thousands separator (default is off)

the output should be:

    
```    
    ┌-----------------┬------┬----------┬-------┬-----┬-----┬-------┐
    │ File            │ #Seq │ Total bp │ Avg   │ N50 │ Min │  Max  │
    ├-----------------┼------┼----------┼-------┼-----┼-----┼-------┤
    │ vir_protein.faa │ 73   │ 14,866   │ 203.6 │ 246 │ 28  │ 1,132 │
    │ dummy.fa        │ 1    │ 8        │ 8.00  │ 8   │ 8   │ 8     │
    └-----------------┴------┴----------┴-------┴-----┴-----┴-------┘
```
    

(We used also our dummy file to check that the tool works as expected!)

[**SeqFu**](https://telatin.github.io/seqfu2/) has a long list of tools, some are modeled after the Unix tools we have seen so far, but with a more powerful syntax and more options.

For example:

*   `seqfu head` will print the first _n_ sequences (also taking 1 every _y_)
*   `seqfu tail` will print the last _n_ sequences
*   `seqfu grep` can select sequences by name or by sequence, including degenerate IUPAC primers and partial matches, or matches in reverse complement
*   `seqfu cat` will concatenate sequences, with filters to remove short/long sequences or truncate/trim them

In addition there are sequence specific tools, such as:

*   `seqfu rc` to reverse complement one or more sequences
*   `seqfu bases` to print the composition of DNA sequences
*   `seqfu qual` to check the quality scores of FASTQ files

And many more...

