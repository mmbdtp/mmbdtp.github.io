---
title: Friday - Session 1

---

# Climb Challenges 

You are given a two data sets:

1. a Staphylococcus aureus bacterium reference genome and some FASTQ files of a closely related mutant strain available at ~/shared-team/2025_training/week1/staph. 

1. a metagenomic sample from the coffee fermentation process available at ~/shared-team/2025_training/week1/metagenome. In this task you will briefly explore the assembled MAGs (you will assemble your own metagenome in week 5). 

 

Tasks: Explore the provide fastq and fasta file before and after assembly 

Think about some echo statements that you can use in your log files. 

Setup:  

Gather information about tools and commands that you will need (e.g. fastqc).  

## Task 1: Staphylococcus QC 

Inspect the quality of the fastq files (Hint: use fastqc) 

What is the reference genome length? (Hint: select the reverse of the headers, remove new line characters and count the bases) 

Using this information and number and length of the reads?  

 

## Task 2: Inspect metagenomic assemblies 

- What is the length of each contig? 

- Create a plot (using R or Python) to compare the contig lengths 

## Task 3: Python plotting recap

- Search the genebank nucleotide website for your fav (short!) sequence and try to display it using the Entrez/dna_features_viewer

- View the same sequence in an actual genome brower (why is this better?)
