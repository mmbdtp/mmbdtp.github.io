---
layout: page
---

# Downloading simulated data 

*Reads are available at [https://zenodo.org/records/10018484](https://zenodo.org/records/10018484)*

You can directly download them with `wget` or similar. 

```
wget -O novel-pathogen-long-reads.v3.fastq.gz https://zenodo.org/records/10018484/files/novel-pathogen-long-reads.v3.fastq.gz?download=1
wget -O novel-pathogen_R1.fastq.gz https://zenodo.org/records/10018484/files/novel-pathogen_R1.fastq.gz?download=1
wget -O novel-pathogen_R2.fastq.gz https://zenodo.org/records/10018484/files/novel-pathogen_R2.fastq.gz?download=1
```

The files are compressed (.gz) FASTQ files. There are three files: 

* Short read R1 - [novel-pathogen_R1.fastq.gz](https://zenodo.org/records/10018484/files/novel-pathogen_R1.fastq.gz?download=1)
* Short read R2 - [novel-pathogen_R2.fastq.gz](https://zenodo.org/records/10018484/files/novel-pathogen_R2.fastq.gz?download=1) 
* Long reads - [novel-pathogen-long-reads.fastq.gz](https://zenodo.org/records/10018484/files/novel-pathogen-long-reads.v3.fastq.gz?download=1)


### Reminder: What is FASTQ

FASTQ format is a text-based format for storing both a biological sequence (usually nucleotide sequence) and its corresponding quality scores. Both the sequence letter and quality score are each encoded with a single ASCII character for brevity.

It was originally developed at the Wellcome Trust Sanger Institute to bundle a FASTA formatted sequence and its quality data, but has become the de facto standard for storing the output of high-throughput sequencing instruments.

### Reminder: What is Gzip 

A GZ file is an archive file compressed by the standard GNU zip (gzip) compression algorithm. It typically contains a single compressed file but may also store multiple compressed files. gzip is primarily used on Unix operating systems for file compression.