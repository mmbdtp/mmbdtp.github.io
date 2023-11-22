---
title: Metagenomics and Python
---

# Week 5: Metagenomics and Python
**Week beginning December 4th 2023**

**Tutors**: **Mark Pallen** ([mark.pallen@quadram.ac.uk](mailto:mark.pallen@quadram.ac.uk)) and **Andrea Telatin** ([andrea.telatin@quadram.ac.uk](mailto:andrea.telatin))

Note that to fit in with room availability, this week's course runs as follows
- Tuesday, Wednesday, Friday all day
- Thursday afternoon

***

### Tuesday 5th Dec
**Venue:** Quadram Institute Room UG55A 9.00am to 5.00pm

*Morning*

- **Introduction to the module**
- **Talk from Mark Pallen**:
  -  _Adventures in Metagenomics Part 1: Tick microbiomes_
- **Talk from Andrea Telatin**
  - _Overview of 16S rRNA gene sequence analysis_
- **Bioinformatics Task**  
- Download dataset from _Rhipicephalus australis_ from _Greay et al study_ [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8906098/]:

  - https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR12461974/SRR12461974
  - https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR12461975/SRR12461975
  - https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR12461939/SRR12461939

- Analyse 16S sequences

*Afternoon*
- **Bioinformatics Task**  
- Download dataset from _Ravi et al tick study_ (https://journals.plos.org/plosntds/article?id=10.1371/journal.pntd.0006805):
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7984991/SRR7984991  (Jade)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7984992/SRR7984992  (Jade)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7984993/SRR7984993  (Wing)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7984994/SRR7984994  (Wing)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7984995/SRR7984995  (Dominic)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7984996/SRR7984996  (Dominic)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7984997/SRR7984997  (Ignatio)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7984998/SRR7984998  (Ignatio)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7984999/SRR7984999  (Raphael)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7985000/SRR7985000  (Raphael)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7985001/SRR7985001  (Charlie)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7985002/SRR7985002  (Charlie)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7985003/SRR7985003  (Ryan)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7985004/SRR7985004  (Ryan)

- K-mer analysis using Kraken and Bracken. Use bacterial reference library and full reference library

***

### Wednesday 6th Dec

**Venue:** Quadram Institute Room UG55A 9.00am to 5.00pm

- **Talk from Mark Pallen**:
  -  _Adventures in Metagenomics Part 2: ICU gut microbiomes_

- **Bioinformatics Tasks**  
- Download dataset from _Ravi et al ICU study_ ([https://journals.plos.org/plosntds/article?id=10.1371/journal.pntd.0006805](https://www.microbiologyresearch.org/content/journal/mgen/10.1099/mgen.0.000293)):
- Everyone should download a normal(ish) gut metagenome: https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR8926093/SRR8926093
- Determine what is unusal and noteworthy in the sample assigned to you
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR8926096/SRR8926096 (Jade)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR8926099/SRR8926099 (Wing)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR8926107/SRR8926107 (Dominic)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR8926114/SRR8926114 (Ignatio)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR8926183/SRR8926183 (Charlie)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR8926188/SRR8926188 (Raphael)
  -  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR8926300/SRR8926300 (Ryan)
- K-mer analysis using Kraken and Bracken. Use bacterial reference library and full reference library
- Binning sequences to create metagenome-assembled genomes (MAGs)

***

### Thursday 7th Dec
**Venue:** Quadram Institute Room UG55A 1.00pm to 5.00pm
- **Talk from Mark Pallen**:
  -  _Adventures with Python and names for bacteria_
- **Talk from Andrea Telatin**
  - _Overview of Python_

- **Bioinformatics Task**
- Write a Python script that translates a DNA sequence into a protein sequence
  - Try it on your own
  - Then ask ChatGPT to help
  - Consider adding extra features to make your script as versatile and powerful and use-friendly as possible

***

### Friday 8th Dec
**Venue:** Quadram Institute Room UG55A 9.00am to 12.00pm; Room UB55C 12.00pm  to 5.00pm

- **Bioinformatics Task**
- Download dataset of metadata on bacterial genomes in GTDB: https://data.gtdb.ecogenomic.org/releases/latest/bac120_metadata.tsv.gz
 -Write a Python script that extracts and analyses relevant information to provide a list of phyla ranked according to average genome size and a list of phyla ranked according to average GC content.
 
***
