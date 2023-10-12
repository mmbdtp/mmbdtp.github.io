---
layout: page
---

# Common file formats

Here are some common genomics sequence file formats, some of which we have mentioned already;. 

| Format               | Description                                            |
|----------------------|--------------------------------------------------------|
| _FASTA_                | A text-based format for representing nucleotide or protein sequences. Each sequence is represented by a header line starting with '>', followed by the sequence data.       |
| _FASTQ_                | A format for representing both nucleotide sequences and their corresponding quality scores. Each record contains a sequence and quality scores in a readable text format.        |
| _SAM (Sequence Alignment/Map)_ | A tab-delimited text format for storing sequence alignment data, often used for mapping short reads to a reference genome.      |
| _BAM (Binary Alignment/Map)_ | A binary version of the SAM format, which is more compact and efficient for large datasets. Used for storing sequence alignment data.        |
| _VCF (Variant Call Format)_ | A text-based format for representing genetic variations, including single nucleotide polymorphisms (SNPs), insertions, deletions, and structural variants.        |
| _BED (Browser Extensible Data)_ | A text-based format for representing genomic intervals, such as regions of interest, gene annotations, and functional elements.        |
| _GFF/GTF (General Feature Format/General Transfer Format)_ | Text-based formats for representing genomic features, including genes, exons, and other structural elements. GFF is often used in older annotations, while GTF is commonly used in more recent annotations.   |
| _FAST5_               | A format used by Oxford Nanopore Technologies (ONT) for storing raw sequencing data produced by their nanopore sequencing platforms. |


## Exercise: Exploring file formats

For each of the examples below. Can you:

* Identify the file format?
* List the common extensions people use?
* Can you define some general rules to determine the file format?

```
>Sequence_1
ATCGGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTA
>Sequence_2
GATCGATCGATCGATCGATCGATCGATCGATCGATCGATCGATCGATCGATCGATCGATC
>Sequence_3
TTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTT
```


## FASTQ

```
@Sequence_1
GATCGGAAGAGCACACGTCTGAACTCCAGTCACATCACGATCTCGTATGC
+
IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII
@Sequence_2
AGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGC
+
JJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJ
@Sequence_3
CGATCGATCGATCGATCGATCGATCGATCGATCGATCGATCGATCGATCG
+
DDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDD
```


## SAM

```sam
@HD	VN:1.0	SO:coordinate



## BAM
```
Wtѭ▒a▒zwc▒Kw▒▒▒Ë▒{Ô▒▒ݳ▒▒▒▒▒▒~}▒pv▒▒m▒֛▒: ▒PJ▒(▒
```

## VCF

## BED


## GFF/GTF


[Back to Programme]({{site.baseurl}}/modules/sequencing/week-2-programme/).
