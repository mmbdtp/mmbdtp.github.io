---
title: Tuesday morning - Assembly and Binning
---


# Tuesday morning 
## Assembly

---


## Objectives 

## Running assemblies

**MEGAHIT** is a powerful, efficient tool for assembling large and complex metagenomic datasets. It is designed for assembling short reads from high-throughput sequencing technologies, such as Illumina, and can handle metagenomic data with mixed organisms or environmental samples. MEGAHIT is particularly well-suited for assembling large-scale data due to its low memory usage and high speed, making it an excellent choice for metagenomic research involving diverse microbial communities.

To save time and resources, for this session, you will each work on a single sample:

 - patient02_day01: Ricardo
 - patient02_day10: Rachael
 - patient04_day10 : Samson
 - patient04_day14: Tabitha
 - patient29: William
 - patient35: Nabina
 - patient36: Chloe

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

 This MEGAHIT command can be broken down as follows:

- **`megahit`**: The command to run the MEGAHIT assembler.
- **`-t 16`**: Specifies the number of threads to use during the assembly, enabling parallel processing. Using 16 threads helps speed up the computation by utilizing more CPU cores.
- **`--12 patient02_day01_R1.fastq.gz,patient02_day01_R2.fastq.gz`**: Indicates paired-end input reads are provided as interleaved files. The reads from the forward (`R1`) and reverse (`R2`) files are specified in a single argument, separated by a comma.
- **`-o megahit_out_patient02_day01`**: Specifies the output directory where MEGAHIT will store the assembly results. In this case, the output folder is named `megahit_out_patient02_day01`.

**Why?**

- **Paired-end reads**: The `--12` option is used when both the forward and reverse reads are in separate files, and you want MEGAHIT to process them as paired data. This helps the assembler use paired-end information for better assembly quality.
- **Output**: The directory `megahit_out_patient02_day01` will contain various files, including the assembled contigs (`final.contigs.fa`), logs, and intermediate files generated during the assembly.

This command will assemble the paired-end reads from `patient02_day01` using 16 CPU threads and store the results in the specified output directory.

---

## De Bruijn grpahs made simple

While that is running, let's watch [this video](https://www.youtube.com/watch?v=OY9Q_rUCGDw) that explains how de Bruijn graphs are used for assembly. 

Then let's comsider what the output of Metahit is telling us.

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



##Quast
Let's take a look at the quality of the assembly using Quast.

```quast.py -o quast_out_patient02_day01 -t 4 -f megahit_out_patient02_day01/final.contigs.fa
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

### Additional Notes:
- **Coverage Depth**: With smaller input files (30-80 MB), coverage depth may be limited, impacting the assembly quality and the contig N50. High-quality assemblies are more likely if coverage depth is around 20x or higher for dominant species.
- **Completeness**: Expect assemblies to be partially complete but representative of the sample's microbial community.
- **Contamination**: Should be low if quality control was performed prior to assembly. Use contamination-checking tools post-assembly for verification.

These figures provide a baseline for what you might expect from simple metagenomes with modest input sizes. For more complex or larger metagenomic datasets, these metrics will vary, often requiring greater resources and more advanced assembly strategies.

---
 
##Coffee break

---


## Metagenomic binning  
Original genomes in the sample can be separated with a process called binning. This process allows separate analysis of each species contained in the metagenome with enough reads to reconstruct a genome. Genomes reconstructed from metagenomic assemblies are called MAGs (Metagenome-Assembled Genomes).
In this process, the assembled contigs from the metagenome will be assigned to different bins (FASTA files that contain certain contigs). Ideally, each bin corresponds to only one original genome (a MAG).


Although an obvious way to separate contigs that correspond to a different species is by their taxonomic assignation, 
there are more reliable methods that do the binning using 
characteristics of the contigs, such as their GC content, the use of tetranucleotides (composition), or their coverage (abundance).

[Maxbin](https://sourceforge.net/projects/maxbin/files/) is a binning algorithm 
that distinguishes between contigs that belong to different bins according to their 
coverage levels and the tetranucleotide frequencies they have.

Let us bin the sample we just assembled. The command for running MaxBin is `run_MaxBin.pl`, and the arguments it needs are the FASTA file of the assembly, the FASTQ with the forward and reverse reads, the output directory, and the name. 

It seems impossible to bin our assembly because the number of marker genes is less than 1. 
We could have expected this as we know it is a small sample.

We will perform the binning process with the other sample from the same study that is larger. We have the assembly precomputed in the `~/dc-workshop/mags/` directory.


SRR8926187 doesnt work
ttagaaaaaaataagtgaag from https://www.ncbi.nlm.nih.gov/nucleotide/NC_018046.1 pulls out Candida mitochondrial contig from megahit_out_SRR8926193/final.contigs.fa.



```
quast.py -o quast -t 4 -f --meta megahit_out_SRR8926297/final.contigs.fa
```

mamba install maxbin

```
#!/bin/bash

# Define the input files
input_file="./first_round/megahit_out_SRR8926123/final.contigs.fa"

# Create output directories
output_dir="analysis_output/SRR8926123"
mkdir -p "$output_dir"

# Step 1: Build Bowtie2 Index
echo "Building bowtie2 index for $input_file..."
bowtie2-build "$input_file" "$output_dir/SRR8926123_contigs_index"

# Step 2: Map Reads to Contigs
echo "Mapping reads for SRR8926123..."
bowtie2 -x "$output_dir/SRR8926123_contigs_index" -1 ./first_round/SRR8926123_1.fastq.gz -2 ./first_round/SRR8926123_2.fastq.gz -S "$output_dir/mapped_reads.sam"

# Step 3: Convert SAM to BAM and Sort
echo "Converting and sorting SAM to BAM for SRR8926123..."
samtools view -S -b "$output_dir/mapped_reads.sam" > "$output_dir/mapped_reads.bam"
samtools sort "$output_dir/mapped_reads.bam" -o "$output_dir/mapped_reads_sorted.bam"

# Step 4: Calculate Coverage Depth
echo "Calculating coverage depth for SRR8926123..."
samtools depth "$output_dir/mapped_reads_sorted.bam" | awk '{sum[$1] += $3; count[$1]++} END {for (contig in sum) print contig, sum[contig]/count[contig]}' > "$output_dir/abundance_data.txt"

# Step 5: Run MaxBin
echo "Running MaxBin for SRR8926123..."
run_MaxBin.pl -contig "$input_file" -abund "$output_dir/abundance_data.txt" -out "$output_dir/maxbin_output" -thread 48

echo "Analysis completed for SRR8926123."

```


---

# Tuesday afternoon
## Questions: The quality of MAGs

Can we trust the quality of our bins only with the given information? 
What else do we want to know about our MAGs to use for further analysis confidently?

## Solution
 
 **completeness** is fundamental to know which data you are working with. If the MAG is incomplete, 
 you can hypothesize that if you did not find something, it is because you do not have a complete genome.
**Genome size** and **GC content** are like genomic fingerprints of taxa, so you can know if you have the taxa you are looking for. Since we are working with the mixed genomes of a community when we try to separate them with binning,  we want to know if we were able to separate them correctly. So we need to measure **contamination** to know if we have only one genome in our bin.


---


## CheckM 

The quality of a MAG is highly dependent on the size of the genome of the species, its abundance 
in the community and the depth at which we sequenced it. Two important things that can be measured to know its quality are completeness (is the MAG a complete genome?) and if it is contaminated (does the MAG contain only one genome?). 

[CheckM](https://github.com/Ecogenomics/CheckM) is an excellent program to see the quality of our MAGs. 
It measures completeness and contamination by counting marker genes in the MAGs. 
The lineage workflow that is a part of CheckM places your bins in a reference tree to know to which lineage it corresponds and to use the appropriate marker genes to estimate the quality parameters. Unfortunately, the lineage workflow uses much memory, so it cannot run on our machines, but we can tell CheckM to use marker genes from Bacteria only to spend less memory. 
This is a less accurate approach, but it can also be advantageous if you want all of your bins analyzed with the same markers. 

We will run the taxonomy workflow specifying the use of markers at the domain level, specific for the rank Bacteria, 
we will specify that our bins are in FASTA format, that they are located in the `MAXBIN` directory 
and that we want our output in the `CHECKM/` directory. 

~~~

$ mkdir CHECKM
$ /shared/team/2024_training/2024_metagenomics/metagenome_sequences/trimmomatic/trimmed_fastqc/megahit_out_SRR8926193$ checkm taxonomy_wf domain Bacteria -x fasta INPUT_BINS/ CHECKM/

~~~

need qa option I think

To have these values in an output that is more usable and shearable, we can now run the quality step of CheckM `checkm qa` 
and make it print the output in a `TSV` table instead of the console. In this step, we can ask CheckM to give us more parameters, like contig number and length.

Ideally, we would like to get only one contig per bin, with a length similar to the genome size of the corresponding taxa. Since this scenario is complicated to obtain, we can use parameters showing how good our assembly is. Here are some of the most common metrics:
If we arrange our contigs by size, from larger to smaller, and divide the whole sequence in half, N50 is the size of the smallest contig in the half that has the larger contigs; and L50 is the number of contigs in this half of the sequence. So we want big N50 and small L50 values for our genomes. Read [What is N50?](https://www.molecularecologist.com/2017/03/29/whats-n50/).

To get the table with these extra parameters, we need to specify the file of the markers that CheckM used in the previous step, `Bacteria.ms`, the name of the output file we want, `quality_JP4D.tsv`, that we want a table `--tab_table`, and the option number 2 `-o 2` is to ask for the extra parameters printed on the table. 
~~~
$  checkm qa CHECKM/Bacteria.ms CHECKM/ --file CHECKM/quality_JP4D.tsv --tab_table -o 2
~~~
{: .bash} 
The table we just made looks like [this](https://github.com/carpentries-incubator/metagenomics/blob/gh-pages/files/quality_JP4D.tsv).
This will be very useful when you need to document or communicate your work. 

The question of how much contamination we can tolerate and how much completeness we need certainly depends on the scientific question being tackled, but in the [CheckM](https://genome.cshlp.org/content/25/7/1043) paper, there are some parameters that we can follow.


DAStool and combining approaches

---


# Thursday morning
Phylogenetic analysis of bins
Dereplication
GTDB-tk

---


# Thursday afternoon
Functional analysis of bins
abricate my_phage.fa >abricate_results.tsv

---


# Friday morning

Long read metagnomics