---
title: Assembly and Binning
---


## Assembly and Binning

---


## Objectives 
1. **Understand the Assembly Process**: Learn how to run the MEGAHIT assembler to create contigs from metagenomic samples and interpret the resulting output, including metrics like the number of contigs, total assembly length, and N50 value.
2. **Gain Practical Skills in Assembly**: Execute MEGAHIT commands for assigned samples, using high-throughput sequencing data to generate assembly outputs and understand the significance of each parameter used in the process.
3. **Familiarize with De Bruijn Graphs**: Understand the fundamental concept of de Bruijn graphs and their role in genome assembly, enhancing the comprehension of how short read data is stitched together into longer contigs.
4. **Perform Quality Assessment with QUAST**: Analyze the quality of the assemblies using QUAST, identify important metrics like GC content and largest contig, and evaluate misassembly rates to assess the reliability of the results.
5. **Introduction to Metagenomic Binning**: Learn how to perform metagenomic binning to separate assembled contigs into bins representing potential genomes, understanding the importance of metrics such as coverage depth and tetranucleotide frequency.
6. **Calculate Coverage and Abundance**: Run backmapping workflows with Bowtie2 and Samtools to align reads back to contigs, calculate coverage depth, and generate abundance data, which is essential for successful binning.
7. **Run MaxBin for Binning**: Implement MaxBin to create MAGs (Metagenome-Assembled Genomes) from assembled contigs and interpret the output to identify genomes of the most abundant species in the metagenomic sample.
8. **Connect Results with Taxonomic Analysis**: Use BLAST to identify sequences within bins and compare findings to taxonomic results obtained from Kraken and MetaPhlAn, reinforcing an understanding of metagenomic data integration. 

---

## Running assemblies

**MEGAHIT** is a powerful, efficient tool for assembling large and complex metagenomic datasets. It is designed for assembling short reads from high-throughput sequencing technologies, such as Illumina, and can handle metagenomic data with mixed organisms or environmental samples. MEGAHIT is particularly well-suited for assembling large-scale data due to its low memory usage and high speed, making it an excellent choice for metagenomic research involving diverse microbial communities.

To save time and resources, for this session, you will each work on a single sample. Decide between yourselves which student takes which sample:

 - patient02_day01
 - patient02_day10
 - patient04_day10
 - patient04_day14
 - patient29
 - patient35
 - patient36

You should run the assmbly on the sample that has been assigned to you, working within your own directory in shared-teams 


Start in your `sequences` directory and pick the command for your sample.

```
megahit -t 16 --12 patient02_day01_R1.fastq.gz,patient02_day01_R2.fastq.gz -o megahit_out_patient02_day01
megahit -t 16 --12 patient02_day10_R1.fastq.gz,patient02_day10_R2.fastq.gz -o megahit_out_patient02_day10
megahit -t 16 --12 patient04_day10_R1.fastq.gz,patient04_day10_R2.fastq.gz -o megahit_out_patient04_day10
megahit -t 16 --12 patient04_day14_R1.fastq.gz,patient04_day14_R2.fastq.gz -o megahit_out_patient04_day14
megahit -t 16 --12 patient29_R1.fastq.gz,patient29_R2.fastq.gz -o megahit_out_patient29
megahit -t 16 --12 patient35_R1.fastq.gz,patient35_R2.fastq.gz -o megahit_out_patient35
megahit -t 16 --12 patient36_R1.fastq.gz,patient36_R2.fastq.gz -o megahit_out_patient36

```
### Explanation:

- **`megahit`**: The command to run the MEGAHIT assembler.
- **`-t 16`**: Specifies the number of threads to use during the assembly, enabling parallel processing. Using 16 threads helps speed up the computation by utilizing more CPU cores.
- **`--12 patient02_day01_R1.fastq.gz,patient02_day01_R2.fastq.gz`**: Indicates paired-end input reads are provided as interleaved files. The reads from the forward (`R1`) and reverse (`R2`) files are specified in a single argument, separated by a comma.
- **`-o megahit_out_patient02_day01`**: Specifies the output directory where MEGAHIT will store the assembly results. In this case, the output folder is named `megahit_out_patient02_day01`.
- **Paired-end reads**: The `--12` option is used when both the forward and reverse reads are in separate files, and you want MEGAHIT to process them as paired data. This helps the assembler use paired-end information for better assembly quality.
- **Output**: The directory `megahit_out_patient02_day01` will contain various files, including the assembled contigs (`final.contigs.fa`), logs, and intermediate files generated during the assembly.

This command will assemble the paired-end reads from `patient02_day01` using 16 CPU threads and store the results in the specified output directory.

---

## De Bruijn graphs made simple

While that is running, let's watch this video that explains how de Bruijn graphs are used for assembly. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/OY9Q_rUCGDw" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

Then let's consider what the output of Metahit is telling us.

```
2024-11-11 11:20:22 - MEGAHIT v1.2.9
2024-11-11 11:20:22 - Using megahit_core with POPCNT and BMI2 support
2024-11-11 11:20:22 - Convert reads to binary library
2024-11-11 11:20:24 - b'INFO  sequence/io/sequence_lib.cpp  :   75 - Lib 0 (/shared/team/2024_training/2024_metagenomics/metagenomics_2024_mark/sequences/patient02_day01_R1.fastq.gz): interleaved, 1158686 reads, 151 max length'
2024-11-11 11:20:27 - b'INFO  sequence/io/sequence_lib.cpp  :   75 - Lib 1 (/shared/team/2024_training/2024_metagenomics/metagenomics_2024_mark/sequences/patient02_day01_R2.fastq.gz): interleaved, 1158686 reads, 151 max length'
2024-11-11 11:20:27 - b'INFO  utils/utils.h                 :  152 - Real: 4.9905\tuser: 2.3200\tsys: 0.3648\tmaxrss: 101708'
2024-11-11 11:20:27 - k-max reset to: 141 
2024-11-11 11:20:27 - Start assembly. Number of CPU threads 16 
2024-11-11 11:20:27 - k list: 21,29,39,59,79,99,119,141 
2024-11-11 11:20:27 - Memory used: 665431953408
2024-11-11 11:20:27 - Extract solid (k+1)-mers for k = 21 
2024-11-11 11:20:44 - Build graph for k = 21 
2024-11-11 11:21:05 - Assemble contigs from SdBG for k = 21
2024-11-11 11:21:59 - Local assembly for k = 21
2024-11-11 11:22:43 - Extract iterative edges from k = 21 to 29 
2024-11-11 11:22:48 - Build graph for k = 29 
2024-11-11 11:23:01 - Assemble contigs from SdBG for k = 29
2024-11-11 11:23:46 - Local assembly for k = 29
2024-11-11 11:24:38 - Extract iterative edges from k = 29 to 39 
2024-11-11 11:24:41 - Build graph for k = 39 
2024-11-11 11:24:52 - Assemble contigs from SdBG for k = 39
2024-11-11 11:25:31 - Local assembly for k = 39
2024-11-11 11:26:46 - Extract iterative edges from k = 39 to 59 
2024-11-11 11:26:49 - Build graph for k = 59 
2024-11-11 11:26:56 - Assemble contigs from SdBG for k = 59
2024-11-11 11:27:21 - Local assembly for k = 59
2024-11-11 11:28:33 - Extract iterative edges from k = 59 to 79 
2024-11-11 11:28:34 - Build graph for k = 79 
2024-11-11 11:28:39 - Assemble contigs from SdBG for k = 79
2024-11-11 11:28:56 - Local assembly for k = 79
2024-11-11 11:30:05 - Extract iterative edges from k = 79 to 99 
2024-11-11 11:30:06 - Build graph for k = 99 
2024-11-11 11:30:10 - Assemble contigs from SdBG for k = 99
2024-11-11 11:30:24 - Local assembly for k = 99
2024-11-11 11:31:28 - Extract iterative edges from k = 99 to 119 
2024-11-11 11:31:29 - Build graph for k = 119 
2024-11-11 11:31:33 - Assemble contigs from SdBG for k = 119
2024-11-11 11:31:45 - Local assembly for k = 119
2024-11-11 11:32:48 - Extract iterative edges from k = 119 to 141 
2024-11-11 11:32:49 - Build graph for k = 141 
2024-11-11 11:32:52 - Assemble contigs from SdBG for k = 141
2024-11-11 11:33:02 - Merging to output final contigs 
2024-11-11 11:33:02 - 32989 contigs, total 21701806 bp, min 270 bp, max 18460 bp, avg 657 bp, N50 723 bp
2024-11-11 11:33:02 - ALL DONE. Time elapsed: 760.569404 seconds 
```

This MEGAHIT output shows the progress and stages of assembling metagenomic data from paired-end reads. 

- The process begins by converting input reads into a binary format and proceeds to iterative assembly steps using different k-mer sizes (from 21 to 141). 
- The log indicates the successful extraction of k-mers, building of assembly graphs, and local assembly processes at each stage. 
- The final output provides a summary of the assembly results: 32,989 contigs were generated, spanning a total of 21,701,806 base pairs, with an average contig length of 657 bp and an N50 of 723 bp, indicating the assembly's quality and contiguity. 
- The process completed in about 761 seconds, demonstrating efficient use of 16 CPU threads.

---

## Quast
Let's take a look at the quality of the assembly using Quast.

```
quast.py -o quast_out_patient02_day01 -t 4 -f megahit_out_patient02_day01/final.contigs.fa
quast.py -o quast_out_patient02_day10 -t 4 -f megahit_out_patient02_day10/final.contigs.fa
quast.py -o quast_out_patient04_day10 -t 4 -f megahit_out_patient04_day10/final.contigs.fa
quast.py -o quast_out_patient04_day14 -t 4 -f megahit_out_patient04_day14/final.contigs.fa
quast.py -o quast_out_patient29 -t 4 -f megahit_out_patient29/final.contigs.fa
quast.py -o quast_out_patient35 -t 4 -f megahit_out_patient35/final.contigs.fa
quast.py -o quast_out_patient36 -t 4 -f megahit_out_patient36/final.contigs.fa
```

*QUAST (Quality Assessment Tool for Genome Assemblies)* provides detailed metrics that help evaluate the quality of genome assemblies. Have a look around at all the output files, including the HTML files. Remember to ••Trust the HTML••!


Here's how to interpret the key outputs and what constitutes a good metagenomics assembly:

1. **Number of Contigs**: Represents the total number of contiguous sequences produced in the assembly. A lower number of contigs generally indicates a more contiguous assembly. Too many contigs suggest fragmentation and may indicate issues such as insufficient read coverage or complex repeat regions.

2. **Total Assembly Length**: The total length of all contigs combined. This should ideally be close to the expected genome size for a pure assembly. In metagenomics, variability is expected due to multiple species.

3. **N50 Value**: A measure of contiguity that indicates the length of the contig at which half of the total assembly length is reached. A higher N50 means longer contigs, which suggests better assembly quality. Good assemblies typically have higher N50 values relative to their complexity and read quality.

4. **Largest Contig**: The length of the longest contig in the assembly. A longer largest contig can be indicative of more successful resolution of genomic structures.

5. **GC Content**: Shows the proportion of guanine and cytosine bases. Consistent GC content across contigs can indicate a homogeneous assembly, while wide variations may point to contamination or different species present.

6. **Misassemblies**: Count and type of detected misassemblies. A high number of misassemblies can imply structural errors in the assembly, which is especially important for metagenomic data as it reflects how accurately the assembly reflects the true genetic makeup.

---

### What Would Count as a Good Metagenomics Assembly:
For relatively simple metagenomic input files as we have here ranging from 30 to 80 MB, which likely represent moderate microbial diversity with lower coverage, here are some expected QUAST figures and interpretations:

1. **Number of Contigs**:
   - **Expected**: 500 – 5,000 contigs.
   - **Interpretation**: A simple metagenome should not result in excessive fragmentation. A lower number of contigs is better, indicating a more contiguous assembly.

2. **Total Assembly Length**:
   - **Expected**: Between 10 Mbp and 50 Mbp.
   - **Interpretation**: This depends on the diversity and size of the genomes in the metagenome. The total length should roughly match the combined genome sizes of the organisms present.

3. **N50 Value**:
   - **Expected**: 500 – 2,000 bp for a moderate assembly.
   - **Interpretation**: Higher N50 values (closer to or above 1,000 bp) are desirable as they suggest longer, more continuous contigs. However, extremely high values may be unrealistic for simple or low-complexity samples with less coverage.

4. **Largest Contig**:
   - **Expected**: 10,000 – 50,000 bp.
   - **Interpretation**: The length of the largest contig should indicate that some longer genomic regions were successfully assembled without fragmentation.

5. **GC Content**:
   - **Expected**: Varies depending on the sample; typically between 40% and 60% for common prokaryotes.
   - **Interpretation**: Consistent GC content is expected across contigs. Large deviations or unusual GC content could indicate contamination or assembly artifacts.

6. **Misassemblies**:
   - **Expected**: Fewer than 50 misassemblies for simple metagenomes.
   - **Interpretation**: A low misassembly count indicates structural reliability. Higher counts suggest potential issues with assembly accuracy.

---

### Additional Notes:
- **Coverage Depth**: With smaller input files (30-80 MB), coverage depth may be limited, impacting the assembly quality and the contig N50. High-quality assemblies are more likely if coverage depth is around 20x or higher for dominant species.
- **Completeness**: Expect assemblies to be partially complete but representative of the sample's microbial community.
- **Contamination**: Should be low if quality control was performed prior to assembly. Use contamination-checking tools post-assembly for verification.

These figures provide a baseline for what you might expect from simple metagenomes with modest input sizes. For more complex or larger metagenomic datasets, these metrics will vary, often requiring greater resources and more advanced assembly strategies.

---
 
## Coffee break

![](https://raw.githubusercontent.com/mmbdtp/mmbdtp.github.io/refs/heads/gh-pages/modules/metagenomics/_posts/DALL·E%202024-11-11%2011.56.44%20-%20A%20whimsical%20scene%20of%20a%20DNA%20double-helix%20being%20assembled%20from%20coffee%20beans%20in%20a%20laboratory%20setting%2C%20with%20a%20few%20PhD%20students%20in%20lab%20coats%20looking%20on%20in%20.webp)

---

## Metagenomic binning  
Genomes in the sample can be recreated with a process called **binning**. This process allows separate analysis of genomes contained in the metagenome so long as there are enough reads to reconstruct each genome. In other words, it will only work for the most abundant species. Genomes reconstructed from metagenomic assemblies are called MAGs (Metagenome-Assembled Genomes). In this process, the assembled contigs from the metagenome are assigned to different bins (FASTA files containing contigs). Ideally, each bin corresponds to only one original genome (a MAG), but it is an error-prone process.

There are various approaches to doing the binning using characteristics of the contigs, such as their GC content, the use of tetranucleotides (composition), or their coverage (abundance).


---

[Maxbin](https://sourceforge.net/projects/maxbin/files/) is a binning algorithm that distinguishes between contigs that belong to different bins according to their coverage levels and the tetranucleotide frequencies they have.

Let us bin the samples we just assembled. But before we can run Maxbin, we have to calculate coverage depth using BowTie and related programs in the process of backmapping 

---


**Backmapping** involves aligning raw reads back to the assembled contigs to determine how well the reads map to different regions of the assembly. This process helps in calculating the coverage depth of each contig, which is directly proportional to the abundance of that contig in the sample.

**How it Works**

- Mapping Reads: The raw sequencing reads are aligned to the contigs using a tool like Bowtie2, producing alignment files (SAM/BAM).
- Calculating Coverage: Tools like samtools depth are used to measure the coverage at each position of the contigs. This coverage data reflects how many reads align to each part of the contig.
- Average Coverage Calculation: The average coverage across each contig is computed to give a single value representing the abundance of that contig.
- Inferring Abundance: Higher average coverage indicates higher abundance of the contig (and the corresponding organism) in the original metagenomic sample.

---


Here's a workflow for all this

```
mkdir -p maxbin_out_patient02_day01
bowtie2-build megahit_out_patient02_day01/final.contigs.fa maxbin_out_patient02_day01/contigs_index
bowtie2 -x maxbin_out_patient02_day01/contigs_index -1 patient02_day01_R1.fastq.gz -2 patient02_day01_R2.fastq.gz -S maxbin_out_patient02_day01/mapped_reads.sam
samtools view -S -b maxbin_out_patient02_day01/mapped_reads.sam > maxbin_out_patient02_day01/mapped_reads.bam
samtools sort maxbin_out_patient02_day01/mapped_reads.bam -o maxbin_out_patient02_day01/mapped_reads_sorted.bam
samtools depth maxbin_out_patient02_day01/mapped_reads_sorted.bam | awk '{sum[$1] += $3; count[$1]++} END {for (contig in sum) print contig, sum[contig]/count[contig]}' > maxbin_out_patient02_day01/abundance_data.txt
run_MaxBin.pl -contig megahit_out_patient02_day01/final.contigs.fa -abund maxbin_out_patient02_day01/abundance_data.txt -out maxbin_out_patient02_day01/maxbin_bins -thread 48

```

This series of commands performs a workflow for mapping reads and generating abundance data to use with MaxBin for metagenomic binning. Here's a brief explanation:

1. **`mkdir -p maxbin_out_patient02_day01`**: Creates an output directory (`maxbin_out_patient02_day01`) to store all subsequent results.

2. **`bowtie2-build megahit_out_patient02_day01/final.contigs.fa maxbin_out_patient02_day01/contigs_index`**: Builds a Bowtie2 index from the contigs produced by MEGAHIT assembly (`final.contigs.fa`). This index is necessary for mapping reads to the contigs.

3. **`bowtie2 -x maxbin_out_patient02_day01/contigs_index -1 patient02_day01_R1.fastq.gz -2 patient02_day01_R2.fastq.gz -S maxbin_out_patient02_day01/mapped_reads.sam`**: Maps the paired-end reads (`R1` and `R2`) to the indexed contigs, producing a SAM file (`mapped_reads.sam`) containing alignment information. This will take some time!

4. **`samtools view -S -b maxbin_out_patient02_day01/mapped_reads.sam > maxbin_out_patient02_day01/mapped_reads.bam`**: Converts the SAM file to a BAM file format, which is a more compact and binary version of the alignment data.

5. **`samtools sort maxbin_out_patient02_day01/mapped_reads.bam -o maxbin_out_patient02_day01/mapped_reads_sorted.bam`**: Sorts the BAM file by coordinates to prepare for downstream analysis.

6. **`samtools depth maxbin_out_patient02_day01/mapped_reads_sorted.bam | awk '{sum[$1] += $3; count[$1]++} END {for (contig in sum) print contig, sum[contig]/count[contig]}' > maxbin_out_patient02_day01/abundance_data.txt`**: Calculates the coverage depth for each contig and uses `awk` to compute the average depth. This generates an abundance data file (`abundance_data.txt`), crucial for binning.

7. **`run_MaxBin.pl -contig megahit_out_patient02_day01/final.contigs.fa -abund maxbin_out_patient02_day01/abundance_data.txt -out maxbin_out_patient02_day01/maxbin_bins -thread 48`**: Runs MaxBin using the contigs and abundance data to bin contigs into potential genomes, leveraging 48 threads for faster processing.

---

## Running the workflow
Run this workflow over your assembly. 

Here's a separate version of the workflow for each of the samples. Copy and run the appropriate set of commands for your sample to complete the workflow.

### Workflow for `patient02_day01`
```bash
mkdir -p maxbin_out_patient02_day01
bowtie2-build megahit_out_patient02_day01/final.contigs.fa maxbin_out_patient02_day01/contigs_index
bowtie2 -x maxbin_out_patient02_day01/contigs_index -1 patient02_day01_R1.fastq.gz -2 patient02_day01_R2.fastq.gz -S maxbin_out_patient02_day01/mapped_reads.sam
samtools view -S -b maxbin_out_patient02_day01/mapped_reads.sam > maxbin_out_patient02_day01/mapped_reads.bam
samtools sort maxbin_out_patient02_day01/mapped_reads.bam -o maxbin_out_patient02_day01/mapped_reads_sorted.bam
samtools depth maxbin_out_patient02_day01/mapped_reads_sorted.bam | awk '{sum[$1] += $3; count[$1]++} END {for (contig in sum) print contig, sum[contig]/count[contig]}' > maxbin_out_patient02_day01/abundance_data.txt
run_MaxBin.pl -contig megahit_out_patient02_day01/final.contigs.fa -abund maxbin_out_patient02_day01/abundance_data.txt -out maxbin_out_patient02_day01/maxbin_out_patient02_day01.bin -thread 48
```

### Workflow for `patient02_day10`
```bash
mkdir -p maxbin_out_patient02_day10
bowtie2-build megahit_out_patient02_day10/final.contigs.fa maxbin_out_patient02_day10/contigs_index
bowtie2 -x maxbin_out_patient02_day10/contigs_index -1 patient02_day10_R1.fastq.gz -2 patient02_day10_R2.fastq.gz -S maxbin_out_patient02_day10/mapped_reads.sam
samtools view -S -b maxbin_out_patient02_day10/mapped_reads.sam > maxbin_out_patient02_day10/mapped_reads.bam
samtools sort maxbin_out_patient02_day10/mapped_reads.bam -o maxbin_out_patient02_day10/mapped_reads_sorted.bam
samtools depth maxbin_out_patient02_day10/mapped_reads_sorted.bam | awk '{sum[$1] += $3; count[$1]++} END {for (contig in sum) print contig, sum[contig]/count[contig]}' > maxbin_out_patient02_day10/abundance_data.txt
run_MaxBin.pl -contig megahit_out_patient02_day10/final.contigs.fa -abund maxbin_out_patient02_day10/abundance_data.txt -out maxbin_out_patient02_day10/maxbin_out_patient02_day10.bin -thread 48
```

### Workflow for `patient04_day10`
```bash
mkdir -p maxbin_out_patient04_day10
bowtie2-build megahit_out_patient04_day10/final.contigs.fa maxbin_out_patient04_day10/contigs_index
bowtie2 -x maxbin_out_patient04_day10/contigs_index -1 patient04_day10_R1.fastq.gz -2 patient04_day10_R2.fastq.gz -S maxbin_out_patient04_day10/mapped_reads.sam
samtools view -S -b maxbin_out_patient04_day10/mapped_reads.sam > maxbin_out_patient04_day10/mapped_reads.bam
samtools sort maxbin_out_patient04_day10/mapped_reads.bam -o maxbin_out_patient04_day10/mapped_reads_sorted.bam
samtools depth maxbin_out_patient04_day10/mapped_reads_sorted.bam | awk '{sum[$1] += $3; count[$1]++} END {for (contig in sum) print contig, sum[contig]/count[contig]}' > maxbin_out_patient04_day10/abundance_data.txt
run_MaxBin.pl -contig megahit_out_patient04_day10/final.contigs.fa -abund maxbin_out_patient04_day10/abundance_data.txt -out maxbin_out_patient04_day10/maxbin_out_patient04_day10.bin -thread 48
```

### Workflow for `patient04_day14`
```bash
mkdir -p maxbin_out_patient04_day14
bowtie2-build megahit_out_patient04_day14/final.contigs.fa maxbin_out_patient04_day14/contigs_index
bowtie2 -x maxbin_out_patient04_day14/contigs_index -1 patient04_day14_R1.fastq.gz -2 patient04_day14_R2.fastq.gz -S maxbin_out_patient04_day14/mapped_reads.sam
samtools view -S -b maxbin_out_patient04_day14/mapped_reads.sam > maxbin_out_patient04_day14/mapped_reads.bam
samtools sort maxbin_out_patient04_day14/mapped_reads.bam -o maxbin_out_patient04_day14/mapped_reads_sorted.bam
samtools depth maxbin_out_patient04_day14/mapped_reads_sorted.bam | awk '{sum[$1] += $3; count[$1]++} END {for (contig in sum) print contig, sum[contig]/count[contig]}' > maxbin_out_patient04_day14/abundance_data.txt
run_MaxBin.pl -contig megahit_out_patient04_day14/final.contigs.fa -abund maxbin_out_patient04_day14/abundance_data.txt -out maxbin_out_patient04_day14/maxbin_out_patient04_day14.bin -thread 48
```

### Workflow for `patient29`
```bash
mkdir -p maxbin_out_patient29
bowtie2-build megahit_out_patient29/final.contigs.fa maxbin_out_patient29/contigs_index
bowtie2 -x maxbin_out_patient29/contigs_index -1 patient29_R1.fastq.gz -2 patient29_R2.fastq.gz -S maxbin_out_patient29/mapped_reads.sam
samtools view -S -b maxbin_out_patient29/mapped_reads.sam > maxbin_out_patient29/mapped_reads.bam
samtools sort maxbin_out_patient29/mapped_reads.bam -o maxbin_out_patient29/mapped_reads_sorted.bam
samtools depth maxbin_out_patient29/mapped_reads_sorted.bam | awk '{sum[$1] += $3; count[$1]++} END {for (contig in sum) print contig, sum[contig]/count[contig]}' > maxbin_out_patient29/abundance_data.txt
run_MaxBin.pl -contig megahit_out_patient29/final.contigs.fa -abund maxbin_out_patient29/abundance_data.txt -out maxbin_out_patient29/maxbin_out_patient29.bin -thread 48
```

### Workflow for `patient35`
```bash
mkdir -p maxbin_out_patient35
bowtie2-build megahit_out_patient35/final.contigs.fa maxbin_out_patient35/contigs_index
bowtie2 -x maxbin_out_patient35/contigs_index -1 patient35_R1.fastq.gz -2 patient35_R2.fastq.gz -S maxbin_out_patient35/mapped_reads.sam
samtools view -S -b maxbin_out_patient35/mapped_reads.sam > maxbin_out_patient35/mapped_reads.bam
samtools sort maxbin_out_patient35/mapped_reads.bam -o maxbin_out_patient35/mapped_reads_sorted.bam
samtools depth maxbin_out_patient35/mapped_reads_sorted.bam | awk '{sum[$1] += $3; count[$1]++} END {for (contig in sum) print contig, sum[contig]/count[contig]}' > maxbin_out_patient35/abundance_data.txt
run_MaxBin.pl -contig megahit_out_patient35/final.contigs.fa -abund maxbin_out_patient35/abundance_data.txt -out maxbin_out_patient35/maxbin_out_patient35.bin -thread 48
```

### Workflow for `patient36`
```bash
mkdir -p maxbin_out_patient36
bowtie2-build megahit_out_patient36/final.contigs.fa maxbin_out_patient36/contigs_index
bowtie2 -x maxbin_out_patient36/contigs_index -1 patient36_R1.fastq.gz -2 patient36_R2.fastq.gz -S maxbin_out_patient36/mapped_reads.sam
samtools view -S -b maxbin_out_patient36/mapped_reads.sam > maxbin_out_patient36/mapped_reads.bam
samtools sort maxbin_out_patient36/mapped_reads.bam -o maxbin_out_patient36/mapped_reads_sorted.bam
samtools depth maxbin_out_patient36/mapped_reads_sorted.bam | awk '{sum[$1] += $3; count[$1]++} END {for (contig in sum) print contig, sum[contig]/count[contig]}' > maxbin_out_patient36/abundance_data.txt
run_MaxBin.pl -contig megahit_out_patient36/final.contigs.fa -abund maxbin_out_patient36/abundance_data.txt -out maxbin_out_patient36/maxbin_out_patient36.bin -thread 48
```

---


## MaxBin outputs
Inside the maxbin_out_patient** directory you will discover your bins. They are labelled  maxbin_out_patient**_bin.001.fasta etc. 

- How many are there?
- How big are they?

Take a look at the maxbin output files (file prefixes will be different for your files)

`maxbin_bins.log`
- Description: A log file that provides information about the MaxBin run, including parameters used, progress updates, warnings, and any issues encountered during the binning process.
- Usage: Helpful for troubleshooting or understanding how MaxBin processed your data.

`maxbin_bins.marker`
- Description: This file contains information about the marker genes used to assign contigs to bins. It lists marker genes identified in each bin and their associated details.
- Usage: Useful for understanding the marker-based strategy MaxBin employed for binning and for verifying the presence of marker genes.

`maxbin_bins.marker_of_each_bin.tar.gz`
- Description: A compressed archive containing the marker genes for each bin. It holds data on the marker genes that were detected and used in the binning process.
- Usage: Can be extracted and examined for a deeper analysis of the marker genes present in each bin.

`maxbin_bins.noclass`
- Description: A file containing contigs that could not be confidently assigned to any bin. These contigs were left unclassified by MaxBin.
- Usage: Important for manual inspection to see if they can be assigned to a bin post-analysis or if they should remain as ambiguous/unbinned.

`maxbin_bins.tooshort`
- Description: Contains contigs that were too short for reliable binning. These were likely filtered out during the binning process because their length did not meet the minimum threshold set by MaxBin.
- Usage: You may review these contigs to see if they hold any significant data that might require manual intervention or reprocessing.

`maxbin_bins.summary`
- Description: A summary report detailing the bins created, including metrics like the number of contigs in each bin, their total length, and other relevant statistics.
- Usage: Provides a quick overview of the results and helps in evaluating the quality and completeness of the bins generated by MaxBin.
 
---


## Time for lunch

![](https://raw.githubusercontent.com/mmbdtp/mmbdtp.github.io/refs/heads/gh-pages/modules/metagenomics/_posts/DALL·E%202024-11-11%2013.09.50%20-%20A%20whimsical%20and%20detailed%20scene%20in%20a%20laboratory%20where%20PhD%20students%20are%20humorously%20eating%20lunch%20out%20of%20metagenomic%20bins.%20The%20students%20are%20seated%20at%20lab%20.webp)


---



