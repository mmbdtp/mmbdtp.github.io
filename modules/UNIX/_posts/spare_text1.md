## A self-paced tutorial

> The goal of this session is to familiarise with some bioinformatics programs working with the command line. This is a pre-amble to the [mapping and sam format tutorial](https://mmbdtp.github.io/modules/unix/week_1_day_4_session_2/).

### Your data, and your output directory

Even a minor mistake (e.g. a typo in a filename you produce) can break a bioinformatic pipeline.
Similarily, this tutorial is unforgiving and require you not to follow it line by line, but to organise your work and being aware of what you are doing.

### Create your output directory

We established that we have a shared directory where we all can write: `/shared/team/`.
Using **ls**, list the directories, and locate one called *day3*.

Create, inside that *day3* directory, a subdirectory named with your first name, all lowercase and with dashes if you need spacers.

:warning: this will be your output directory. Unless otherwise specified, save the output files of your exercises here, for today. I will refer to this directory as *your output directory*.


### Our reference genome

> This is the reference genome we will map short reads against in a future tutorial.

We will work with a genome. The reference filename is `vir_genomic.fna`.

1. Use **find** to locate the reference genome.
2. Use **grep** to list the sequences present in the genome (and see what bug we are dealing with)

To check how big is the genome, we can use [seqfu stats](https://telatin.github.io/seqfu2/tools/stats.html). It is installed in the environment we already used, but to practice creating conda environments:

1. Use **mamba** to create and environment called "fastq", containing the following tools:
    - seqfu
    - seqkit
    - fastp
2. Using **seqfu stats** identify the genome size.
3. Create a file called `genome.stats` in your output directory having the first and third column of the seqfu stats output.

<details>
<summary>Notes &amp; Hints</summary>
<li>Remember to activate the environment you need with `conda activate NAME`</li>
</details>

### The GFF format

> The genome annotation can be used, for example, to make sense of genomic variations, or quantify gene expressionin  RNA-Seq 

The genome has been annotated. This means that using some methodologies (usually automated, but manual curation is possible!) the coding regions (CDSs) have been identified and located, and when possible a function has been associated to the gene products.

Commonly used formats to store the annotation are:
* [BED format](https://en.wikipedia.org/wiki/BED_(file_format))
* [GFF3 format](https://www.ensembl.org/info/website/upload/gff.html?redirect=no)

Please, check the two pages to understand how these formats are, then locate the file ending with "gff" located in our learn_bash directory.

1. Using **less -S** inspect the file
1. Combining **grep** and **wc**:
	- How many header lines are there
    - How many non-header lines are there
3. The third column is made of labels (see the docs). Use a pipeline to identify how many unique labels are present in this file. 
    - Save the output in your output directory calling it `labels-gff.list`
4. Using a pipeline, now *count* the number of occurrences, possibly sorting the output by number.
    - Save the output in your output directory calling it `labels-gff.counts.list`

<details>
<summary>Notes &amp; Hints</summary>
<li>`grep "#"` will select header lines, the invert match is done with `-v`</li>
<li>Remember we used `sort | uniq` to categorize? We can use `sort | uniq -c` to quantitate, and finally sort numerically the output with a further `sort -n`</li>

</details>

### The raw reads

> This section deals with files produced by Illumina sequencing of genomic DNA.

Some isolates of the organism of interest have been sequences. Locate a directory called reads in our learn_bash directory (**find** can be instructed to only locate directories!)

1. List the files contained in that directory, and notice the extension they have. Have they been compressed?
2. Use **[seqfu stats](https://telatin.github.io/seqfu2/tools/stats.html)** to check the number of reads in each file. If you restrict the output on columns 1 and 2 you will have a clearer output.
    - Save the stats (possibly the clearer two columns version!) in your output directory as `reads.stats`
3. As you can notice, some files contain the same number of sequences. 
    - We briefly mentioned why, did you [recall it](https://thesequencingcenter.com/knowledge-base/what-are-paired-end-reads/)?
    - Some samples have more reads than others, and we refer to this as *[average coverage](https://en.wikipedia.org/wiki/Coverage_(genetics))* (or sequencing depth).

<details>
<summary>Notes &amp; Hints</summary>
<li>To find directories `find -type d`</li>
<li>Note that seqfu can read gzipped files. This is not the case for most unix commands, like "wc". To use wc we should stream the output of gzip (-d to decompress, -c to send to standard output) `gzip -dc FILE.gz | wc -l`</li>
</details>