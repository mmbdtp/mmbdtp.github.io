---
title: Day3 DIY
---

# A self-paced tutorial

```note
The goal of this session is to familiarise with some bioinformatics programs
working with the command line and conda
```

## Your data, and your output directory

Even a minor mistake (e.g. a typo in a filename you produce) can break a bioinformatic pipeline.
Similarily, this tutorial is unforgiving and require you not to follow it line by line, but to organise your work and being aware of what you are doing.

### Create your output directory

We established that we have a shared directory where we all can write: `/shared/team/`.
Using **ls**, list the directories, and locate one called *day3*.

Create, inside that *day3* directory, a subdirectory named with your first name, all lowercase and with dashes if you need spacers.

:warning: this will be your output directory. Unless otherwise specified, save the output files of your exercises here, for today. I will refer to this directory as *your output directory*.


### Our reference genome

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

### The GFF format

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




