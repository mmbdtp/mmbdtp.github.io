---
title: Quality control for short reads
---

In this section, we will be using some example data to assess the quality of short reads. We will use two tools, FASTQE and FASTQC. We will also use some tools to trim poor quality reads (or parts of reads).

> Remember, the pKP1-NDM-1 reads are simulated reads, with minimal error. These are effectively "perfect" and will not be representative of real data. We can use this to compare with problematic data (female_oral2.fastq.gz)

### Where is the example data? 

We will use some example data, found in `~/shared-team/week2/short-read-qc`. Copy it to your home directory. 

```bash
cp -r ~/shared-team/week2/short-read-qc ~/
cd ~/short-read-qc
```

**If you are not using a CLIMB-BIG-DATA notebook, you will need to download the data.**
```
wget -O female_oral2.fastq.gz https://zenodo.org/record/3977236/files/female_oral2.fastq-4143.gz?download=1
wget -O pKP1-NDM-1_R1.fastq.gz https://zenodo.org/records/10018484/files/pKP1-NDM-1_R1.fastq.gz?download=1
wget -O pKP1-NDM-1_R2.fastq.gz https://zenodo.org/records/10018484/files/pKP1-NDM-1_R2.fastq.gz?download=1
```

**female_oral2.fastq.gz**: This is a microbiome sample (16S) from a snake [Jacques et al. 2021.](https://qubeshub.org/community/groups/coursesource/publications?id=2727&v=1).

### Required software

If you are not using a CLIMB-BIG-DATA notebook, you may need to install some software. This is how to do it via conda: 

```
conda install fastqe fastqc -y
```

## Assess quality with FASTQE 

To take a look at sequence quality along all sequences, we can use FASTQE. It is an open-source tool that provides a simple and fun way to quality control raw sequence data and print them as emoji. You can use it to give a quick impression of whether your data has any problems of which you should be aware before doing any further analysis.

Rather than looking at quality scores for each individual read, FASTQE looks at quality collectively across all reads within a sample and can calculate the mean for each nucleotide position along the length of the reads. The quality scores are explained in the [FASTQE documentation](https://github.com/fastqe/fastqe#scale). 

### Exercise 1: Run FASTQE

**Run FASTQE on the example data, pKP1-NDM-1_R1.fastq.gz and female_oral2.fastq.gz. One at a time. Which data has the better average quality?**

**Which portion of read has the lower average quality, and for which file?**

**What is the lowest mean score in female_oral2.fastq.gz?**

[Answers to exercise 1](/seq-data/short-read-qc-answers)

> FASTQE is a fun tool, but we use something more ... comprehensive ... in practice.

## Assess quality with FASTQC

An additional or alternative way we can check sequence quality is with [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/). It provides a modular set of analyses which you can use to check whether your data has any problems of which you should be aware before doing any further analysis. We can use it, for example, to assess whether there are known adapters present in the data. We'll run it on the FASTQ files.

To run FastQC, open your terminal or command prompt and navigate to the directory where your data files are located. Then, use the fastqc command followed by the path to your data files. For example:

```bash
fastqc file1.fastq file2.fastq
```

You can also use wildcards to analyze multiple files at once, like this:
```bash
fastqc *.fastq
```

FastQC will process each file and generate an HTML report for each. *Are you able to open the report via the notebook file browser?*.  The reports contain various quality control metrics and visualizations. See the help via:

```bash
fastqc --help 
```


> FASTQC will also work for long reads. 

### Exercise 2: Run FASTQC

* Run FASTQC on female_oral2.fastq.gz.
* Run FASTQC on pKP1-NDM-1_R1.fastq.gz and pKP1-NDM-1_R2.fastq.gz together.
* Review and compare the HTML reports.

**Which metrics are a major difference between the two reports?**

**What is the parts of the report are missing for pKP1-NDM? Can you explain why?**

**Review each metric for female_oral2.fastq.gz, what part of each plot suggests there is a problem?**

> Remember, the pKP1-NDM-1 reads are simulated reads, with minimal error. These are effectively "perfect" and will not be representative of real data. We can use this to compare with problematic data (female_oral2.fastq.gz)

**female_oral2.fastq.gz data looks terrible, we should probably resequence it, but if we had to; how could we improve the quality?**

[Answers to exercise 2](/seq-data/short-read-qc-answers)

### Citation

Some of this material was adapted from:

* Bérénice Batut, Maria Doyle, Alexandre Cormier, Anthony Bretaudeau, Laura Leroi, Erwan Corre, Stéphanie Robin, Erasmus+ Programme, Cameron Hyde, Quality Control (Galaxy Training Materials). https://training.galaxyproject.org/training-material/topics/sequence-analysis/tutorials/quality-control/tutorial.html Online; accessed Wed Oct 18 2023
* Hiltemann, Saskia, Rasche, Helena et al., 2023 Galaxy Training: A Powerful Framework for Teaching! PLOS Computational Biology 10.1371/journal.pcbi.1010752


[Next: Long read QC]({{site.baseurl}}/modules/sequencing/long-read-qc/).

[Back to Programme]({{site.baseurl}}/modules/sequencing/week-2-programme/).

