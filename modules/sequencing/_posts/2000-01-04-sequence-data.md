---
title: Revisting sequence file formats
---

## Revisting sequence file formats


### Representing nucleotides 
During sequencing, the nucleotide bases in a DNA or RNA sample (library) are determined by the sequencer. For each fragment in the library, a sequence is generated, also called a read, which is simply a succession of nucleotides. The sequence of a read is represented by a string of letters, where each letter represents a nucleotide base. The most common nucleotide bases are adenine (A), cytosine (C), guanine (G), and thymine (T). In RNA, thymine is replaced by uracil (U). 

While we talk about file "formats" for sequencing data these are not the same as file formats for propperity programs such as MS Word or binary file formats. These are simply text files with a particular structure. This means that you can open these files and read them as plain text even if the file extension is `.fastq`, `.fasta` and so on. 

Sequencing instruments may also give ambiguous signals, which are represented by the letters R, Y, S, W, K, M, B, D, H, V, and N. This convention was set by the International Union of Pure and Applied Chemistry (IUPAC) in 1985. These letters represent the following combinations of nucleotides:

| IUPAC nucleotide code | Base                |
|-----------------------|---------------------|
| A                     | Adenine             |
| C                     | Cytosine            |
| G                     | Guanine             |
| T (or U)              | Thymine (or Uracil) |
| R                     | A or G              |
| Y                     | C or T              |
| S                     | G or C              |
| W                     | A or T              |
| K                     | G or T              |
| M                     | A or C              |
| B                     | C or G or T         |
| D                     | A or G or T         |
| H                     | A or C or T         |
| V                     | A or C or G         |
| N                     | any base            |
| . or -                | gap                 |

> N means the base could not be determined. This is different from a gap, which means the base is not present in the sequence.

Read [Johnson 2010](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2865858/) for more details. 


### Common file formats

Here are some common genomics sequence file formats, some of which we have mentioned already;. 

| Format               | Description                                            |
|----------------------|--------------------------------------------------------|
| FASTA                | A text-based format for representing nucleotide or protein sequences. Each sequence is represented by a header line starting with '>', followed by the sequence data.       |
| FASTQ                | A format for representing both nucleotide sequences and their corresponding quality scores. Each record contains a sequence and quality scores in a readable text format.        |
| SAM (Sequence Alignment/Map) | A tab-delimited text format for storing sequence alignment data, often used for mapping short reads to a reference genome.      |
| BAM (Binary Alignment/Map) | A binary version of the SAM format, which is more compact and efficient for large datasets. Used for storing sequence alignment data.        |
| VCF (Variant Call Format) | A text-based format for representing genetic variations, including single nucleotide polymorphisms (SNPs), insertions, deletions, and structural variants.        |
| BED (Browser Extensible Data) | A text-based format for representing genomic intervals, such as regions of interest, gene annotations, and functional elements.        |
| GFF/GTF (General Feature Format/General Transfer Format) | Text-based formats for representing genomic features, including genes, exons, and other structural elements. GFF is often used in older annotations, while GTF is commonly used in more recent annotations.   |
| FAST5               | A format used by Oxford Nanopore Technologies (ONT) for storing raw sequencing data produced by their nanopore sequencing platforms. |


### Representing errors or uncertainty  in sequencing data

Modern sequencing technologies can generate a massive number of sequence reads in a single experiment. However, no sequencing technology is perfect, and each instrument will generate different types and amount of errors, such as incorrect nucleotides being called. These wrongly called bases are due to the technical limitations of each sequencing platform.

Therefore, it is necessary to understand, identify and exclude error-types that may impact the interpretation of downstream analysis. Sequence quality control is therefore an essential first step in your analysis. Catching errors early saves time later on.
