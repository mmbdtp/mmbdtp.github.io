---
title: Revisting sequence file formats
---

## Revisting sequence file formats

During sequencing, the nucleotide bases in a DNA or RNA sample (library) are determined by the sequencer. For each fragment in the library, a sequence is generated, also called a read, which is simply a succession of nucleotides.

Modern sequencing technologies can generate a massive number of sequence reads in a single experiment. However, no sequencing technology is perfect, and each instrument will generate different types and amount of errors, such as incorrect nucleotides being called. These wrongly called bases are due to the technical limitations of each sequencing platform.

Therefore, it is necessary to understand, identify and exclude error-types that may impact the interpretation of downstream analysis. Sequence quality control is therefore an essential first step in your analysis. Catching errors early saves time later on.

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
| PDB (Protein Data Bank) | A file format for representing the three-dimensional structures of biological macromolecules, such as proteins and nucleic acids.         |
| CRAM (Compressed SAM) | A compressed and indexed version of the SAM format, designed to reduce storage space while retaining accessibility to sequence alignment data.   |
| FAST5               | A format used by Oxford Nanopore Technologies (ONT) for storing raw sequencing data produced by their nanopore sequencing platforms. |


