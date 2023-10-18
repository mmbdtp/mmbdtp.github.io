---
layout: page
---

While we talk about file "formats" for sequencing data these are not the same as file formats for propperity programs such as MS Word or binary file formats. These are simply text files with a particular structure. This means that you can open these files and read them as plain text even if the file extension is `.fastq`, `.fasta` and so on. 

# Common file formats

Here are some common genomics sequence file formats, some of which we have mentioned already;. 

| Format               | Description                                            |
|----------------------|--------------------------------------------------------|
| _FASTA_                | A text-based format for representing nucleotide or protein sequences. Each sequence is represented by a header line starting with '>', followed by the sequence data.       |
| _FASTQ_                | A format for representing both nucleotide sequences and their corresponding quality scores. Each record contains a sequence and quality scores in a readable text format.        |
| _SAM (Sequence Alignment/Map)_ | A tab-delimited text format for storing sequence alignment data, often used for mapping short reads to a reference genome.      |
| _BAM (Binary Alignment/Map)_ | A binary version of the SAM format, which is more compact and efficient for large datasets. Used for storing sequence alignment data.        |
| _VCF (Variant Call Format)_ | A text-based format for representing genetic variations, including single nucleotide polymorphisms (SNPs), insertions, deletions, and structural variants.        |
| _BCF (Binary Call Format)_ |  A binary version of the VCF format, which is more compact and efficient for large datasets.        |
| _BED (Browser Extensible Data)_ | A text-based format for representing genomic intervals, such as regions of interest, gene annotations, and functional elements.        |
| _GFF/GTF (General Feature Format/General Transfer Format)_ | Text-based formats for representing genomic features, including genes, exons, and other structural elements. GFF is often used in older annotations, while GTF is commonly used in more recent annotations.   |
| _FAST5_               | A format used by Oxford Nanopore Technologies (ONT) for storing raw sequencing data produced by their nanopore sequencing platforms. |


## Exercise: Exploring file formats

For each of the examples below. Can you:

* Identify the file format?
* List the common extensions people use?
* Can you define some general rules to determine the file format?

[Next: Using tar and gzip](/seq-data/fastq-in-detail)

[Back to Programme]({{site.baseurl}}/modules/sequencing/week-2-programme/).
