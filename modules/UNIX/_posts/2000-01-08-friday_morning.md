---
title: Friday morning — Finding sequence variants
---

# Finding sequence variants

**Credit**: this material has been adapted from the [Data Carpentry](https://datacarpentry.org) lesson [Data Wrangling and Processing for Genomics](https://datacarpentry.org/wrangling-genomics/). The adaptations principally include making sure commands work on the CLIMB-BIG-DATA Notebook servers. All Carpentries instructional material, and therefore all material here, is made available under the Creative Commons Attribution license. See [https://github.com/datacarpentry/wrangling-genomics/blob/main/LICENSE.md](https://github.com/datacarpentry/wrangling-genomics/blob/main/LICENSE.md).


---



## How do I find sequence variants between my sample and a reference genome?

We mentioned before that we are working with files from a long-term evolution study of an *E. coli* population (designated Ara-3). Now that we have looked at our data to make sure that it is high quality, and removed low-quality base calls, we can perform variant calling to see how the population changed over time. We care how this population changed relative to the original population, *E. coli* strain REL606. Therefore, we will align each of our samples to the *E. coli* REL606 reference genome, and see what differences exist in our reads versus the genome.

---

## Alignment to a reference genome

![](https://datacarpentry.org/wrangling-genomics/fig/variant_calling_workflow_align.png){alt='workflow\_align'}

We perform read alignment or mapping to determine where in the genome our reads originated from. There are a number of tools to
choose from and, while there is no gold standard, there are some tools that are better suited for particular NGS analyses. We will be
using the [Burrows Wheeler Aligner (BWA)](https://bio-bwa.sourceforge.net/), which is a software package for mapping low-divergent
sequences against a large reference genome.

The alignment process consists of two steps:

1. Indexing the reference genome
2. Aligning the reads to the reference genome

---


## Setting up

First we download the reference genome for *E. coli* REL606. Although we could copy or move the file with `cp` or `mv`, most genomics workflows begin with a download step, so we will practice that here.

```bash
$ cd ~/dc_workshop
$ mkdir -p data/ref_genome
$ curl -L -o data/ref_genome/ecoli_rel606.fasta.gz ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/017/985/GCA_000017985.1_ASM1798v1/GCA_000017985.1_ASM1798v1_genomic.fna.gz
$ gunzip data/ref_genome/ecoli_rel606.fasta.gz
```

We will also download a set of trimmed FASTQ files to work with. These are small subsets of our real trimmed data,
and will enable us to run our variant calling workflow quite quickly.

```bash
$ curl -L -o sub.tar.gz https://ndownloader.figshare.com/files/14418248
$ tar xvf sub.tar.gz
$ mv sub/ ~/dc_workshop/data/trimmed_fastq_small
```

You will also need to create directories for the results that will be generated as part of this workflow. We can do this in a single
line of code, because `mkdir` can accept multiple new directory
names as input.

```bash
$ mkdir -p results/sam results/bam results/bcf results/vcf
```

---


### Index the reference genome

Our first step is to index the reference genome for use by BWA. Indexing allows the aligner to quickly find potential alignment sites for query sequences in a genome, which saves time during alignment. Indexing the reference only has to be run once. The only reason you would want to create a new index is if you are working with a different reference genome or you are using a different tool for alignment.

You need to install `bwa` using conda! Then you can run it.

```bash
$ bwa index data/ref_genome/ecoli_rel606.fasta
```

While the index is created, you will see output that looks something like this:

```output
[bwa_index] Pack FASTA... 0.04 sec
[bwa_index] Construct BWT for the packed sequence...
[bwa_index] 1.05 seconds elapse.
[bwa_index] Update BWT... 0.03 sec
[bwa_index] Pack forward-only FASTA... 0.02 sec
[bwa_index] Construct SA from BWT and Occ... 0.57 sec
[main] Version: 0.7.17-r1188
[main] CMD: bwa index data/ref_genome/ecoli_rel606.fasta
[main] Real time: 1.765 sec; CPU: 1.715 sec
```

---


### Align reads to reference genome

The alignment process consists of choosing an appropriate reference genome to map our reads against and then deciding on an
aligner. We will use the BWA-MEM algorithm, which is the latest and is generally recommended for high-quality queries as it
is faster and more accurate.

An example of what a `bwa` command looks like is below. This command will not run, as we do not have the files `ref_genome.fa`, `input_file_R1.fastq`, or `input_file_R2.fastq`.

```bash
$ bwa mem ref_genome.fasta input_file_R1.fastq input_file_R2.fastq > output.sam
```

Have a look at the [bwa options page](https://bio-bwa.sourceforge.net/bwa.shtml). While we are running bwa with the default
parameters here, your use case might require a change of parameters. *NOTE: Always read the manual page for any tool before using
and make sure the options you use are appropriate for your data.*

We are going to start by aligning the reads from just one of the samples in our dataset (`SRR2584866`). Later, we will be iterating this whole process on all of our sample files.

```bash
$ bwa mem data/ref_genome/ecoli_rel606.fasta data/trimmed_fastq_small/SRR2584866_1.trim.sub.fastq data/trimmed_fastq_small/SRR2584866_2.trim.sub.fastq > results/sam/SRR2584866.aligned.sam
```

You will see output that starts like this:

```output
[M::bwa_idx_load_from_disk] read 0 ALT contigs
[M::process] read 77446 sequences (10000033 bp)...
[M::process] read 77296 sequences (10000182 bp)...
[M::mem_pestat] # candidate unique pairs for (FF, FR, RF, RR): (48, 36728, 21, 61)
[M::mem_pestat] analyzing insert size distribution for orientation FF...
[M::mem_pestat] (25, 50, 75) percentile: (420, 660, 1774)
[M::mem_pestat] low and high boundaries for computing mean and std.dev: (1, 4482)
[M::mem_pestat] mean and std.dev: (784.68, 700.87)
[M::mem_pestat] low and high boundaries for proper pairs: (1, 5836)
[M::mem_pestat] analyzing insert size distribution for orientation FR...
```

---


#### SAM/BAM format

The [SAM file](https://genome.sph.umich.edu/wiki/SAM),
is a tab-delimited text file that contains information for each individual read and its alignment to the genome. While we do not
have time to go into detail about the features of the SAM format, the paper by
[Heng Li et al.](https://bioinformatics.oxfordjournals.org/content/25/16/2078.full) provides a lot more detail on the specification.

**The compressed binary version of SAM is called a BAM file.** We use this version to reduce size and to allow for *indexing*, which enables efficient random access of the data contained within the file.

The file begins with a **header**, which is optional. The header is used to describe the source of data, reference sequence, method of
alignment, etc., this will change depending on the aligner being used. Following the header is the **alignment section**. Each line
that follows corresponds to alignment information for a single read. Each alignment line has **11 mandatory fields** for essential
mapping information and a variable number of other fields for aligner specific information. An example entry from a SAM file is
displayed below with the different fields highlighted.

![](https://datacarpentry.org/wrangling-genomics/fig/sam_bam.png)

![](https://datacarpentry.org/wrangling-genomics/fig/sam_bam3.png)

We will convert the SAM file to BAM format using the `samtools` program.

First install samtools!

Then with the `view` command and tell this command that the input is in SAM format (`-S`) and to output BAM format (`-b`):

```bash
$ samtools view -S -b results/sam/SRR2584866.aligned.sam > results/bam/SRR2584866.aligned.bam
```

---


### Sort BAM file by coordinates

Next we sort the BAM file using the `sort` command from `samtools`. `-o` tells the command where to write the output.

```bash
$ samtools sort -o results/bam/SRR2584866.aligned.sorted.bam results/bam/SRR2584866.aligned.bam 
```

Our files are pretty small, so we will not see this output. If you run the workflow with larger files, you will see something like this:

```output
[bam_sort_core] merging from 2 files...
```

SAM/BAM files can be sorted in multiple ways, e.g. by location of alignment on the chromosome, by read name, etc. It is important to be aware that different alignment tools will output differently sorted SAM/BAM, and different downstream tools require differently sorted alignment files as input.

You can use samtools to learn more about this bam file as well.

```bash
samtools flagstat results/bam/SRR2584866.aligned.sorted.bam
```

This will give you the following statistics about your sorted bam file:

```output
351169 + 0 in total (QC-passed reads + QC-failed reads)
0 + 0 secondary
1169 + 0 supplementary
0 + 0 duplicates
351103 + 0 mapped (99.98% : N/A)
350000 + 0 paired in sequencing
175000 + 0 read1
175000 + 0 read2
346688 + 0 properly paired (99.05% : N/A)
349876 + 0 with itself and mate mapped
58 + 0 singletons (0.02% : N/A)
0 + 0 with mate mapped to a different chr
0 + 0 with mate mapped to a different chr (mapQ>=5)
```

---


## Variant calling

A variant call is a conclusion that there is a nucleotide difference vs. some reference at a given position in an individual genome
or transcriptome, often referred to as a Single Nucleotide Variant (SNV). The call is usually accompanied by an estimate of
variant frequency and some measure of confidence. Similar to other steps in this workflow, there are a number of tools available for
variant calling. In this workshop we will be using `bcftools`, but there are a few things we need to do before actually calling the
variants.

---


![](https://datacarpentry.org/wrangling-genomics/fig/variant_calling_workflow.png){alt='workflow'}


---


#### Step 1: Calculate the read coverage of positions in the genome

Do the first pass on variant calling by counting read coverage with
[bcftools](https://samtools.github.io/bcftools/bcftools.html). 

Install bcftools

We will then use the command `mpileup`. The flag `-O b` tells bcftools to generate a bcf format output file, `-o` specifies where to write the output file, and `-f` flags the path to the reference genome:

```bash
$ bcftools mpileup -O b -o results/bcf/SRR2584866_raw.bcf \
-f data/ref_genome/ecoli_rel606.fasta results/bam/SRR2584866.aligned.sorted.bam 
```

```output
[mpileup] 1 samples in 1 input files
```

We have now generated a file with coverage information for every base.

---


#### Step 2: Detect the single nucleotide variants (SNVs)

Identify SNVs using bcftools `call`. We have to specify ploidy with the flag `--ploidy`, which is one for the haploid *E. coli*. `-m` allows for multiallelic and rare-variant calling, `-v` tells the program to output variant sites only (not every site in the genome), and `-o` specifies where to write the output file:

```bash
$ bcftools call --ploidy 1 -m -v -o results/vcf/SRR2584866_variants.vcf results/bcf/SRR2584866_raw.bcf 
```

---


#### Step 3: Filter and report the SNV variants in variant calling format (VCF)

Filter the SNVs for the final output in VCF format, using `vcfutils.pl`

```bash
$ vcfutils.pl varFilter results/vcf/SRR2584866_variants.vcf  > results/vcf/SRR2584866_final_variants.vcf
```

---



### Filtering

The `vcfutils.pl varFilter` call filters out variants that do not meet minimum quality default criteria, which can be changed through
its options. Using `bcftools` we can verify that the quality of the variant call set has improved after this filtering step by
calculating the ratio of [transitions(TS)](https://en.wikipedia.org/wiki/Transition_%28genetics%29) to
[transversions (TV)](https://en.wikipedia.org/wiki/Transversion) ratio (TS/TV),
where transitions should be more likely to occur than transversions:

With the command:
```bash
$ bcftools stats results/vcf/SRR2584866_variants.vcf | grep TSTV
```

Output will be:

```output
# TSTV, transitions/transversions:
# TSTV	[2]id	[3]ts	[4]tv	[5]ts/tv	[6]ts (1st ALT)	[7]tv (1st ALT)	[8]ts/tv (1st ALT)
TSTV	0	628	58	10.83	628	58	10.83
```

With the command:
```bash
$ bcftools stats results/vcf/SRR2584866_final_variants.vcf | grep TSTV
```

Output will be:
```output
# TSTV, transitions/transversions:
# TSTV	[2]id	[3]ts	[4]tv	[5]ts/tv	[6]ts (1st ALT)	[7]tv (1st ALT)	[8]ts/tv (1st ALT)
TSTV	0	621	54	11.50	621	54	11.50
```

### Explore the VCF format:

```bash
$ less -S results/vcf/SRR2584866_final_variants.vcf
```

You will see the header (which describes the format), the time and date the file was
created, the version of bcftools that was used, the command line parameters used, and
some additional information:

```output
##fileformat=VCFv4.2
##FILTER=<ID=PASS,Description="All filters passed">
##bcftoolsVersion=1.8+htslib-1.8
##bcftoolsCommand=mpileup -O b -o results/bcf/SRR2584866_raw.bcf -f data/ref_genome/ecoli_rel606.fasta results/bam/SRR2584866.aligned.sorted.bam
##reference=file://data/ref_genome/ecoli_rel606.fasta
##contig=<ID=CP000819.1,length=4629812>
##ALT=<ID=*,Description="Represents allele(s) other than observed.">
##INFO=<ID=INDEL,Number=0,Type=Flag,Description="Indicates that the variant is an INDEL.">
##INFO=<ID=IDV,Number=1,Type=Integer,Description="Maximum number of reads supporting an indel">
##INFO=<ID=IMF,Number=1,Type=Float,Description="Maximum fraction of reads supporting an indel">
##INFO=<ID=DP,Number=1,Type=Integer,Description="Raw read depth">
##INFO=<ID=VDB,Number=1,Type=Float,Description="Variant Distance Bias for filtering splice-site artefacts in RNA-seq data (bigger is better)",Version=
##INFO=<ID=RPB,Number=1,Type=Float,Description="Mann-Whitney U test of Read Position Bias (bigger is better)">
##INFO=<ID=MQB,Number=1,Type=Float,Description="Mann-Whitney U test of Mapping Quality Bias (bigger is better)">
##INFO=<ID=BQB,Number=1,Type=Float,Description="Mann-Whitney U test of Base Quality Bias (bigger is better)">
##INFO=<ID=MQSB,Number=1,Type=Float,Description="Mann-Whitney U test of Mapping Quality vs Strand Bias (bigger is better)">
##INFO=<ID=SGB,Number=1,Type=Float,Description="Segregation based metric.">
##INFO=<ID=MQ0F,Number=1,Type=Float,Description="Fraction of MQ0 reads (smaller is better)">
##FORMAT=<ID=PL,Number=G,Type=Integer,Description="List of Phred-scaled genotype likelihoods">
##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
##INFO=<ID=ICB,Number=1,Type=Float,Description="Inbreeding Coefficient Binomial test (bigger is better)">
##INFO=<ID=HOB,Number=1,Type=Float,Description="Bias in the number of HOMs number (smaller is better)">
##INFO=<ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
##INFO=<ID=AN,Number=1,Type=Integer,Description="Total number of alleles in called genotypes">
##INFO=<ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
##INFO=<ID=MQ,Number=1,Type=Integer,Description="Average mapping quality">
##bcftools_callVersion=1.8+htslib-1.8
##bcftools_callCommand=call --ploidy 1 -m -v -o results/bcf/SRR2584866_variants.vcf results/bcf/SRR2584866_raw.bcf; Date=Tue Oct  9 18:48:10 2018
```

Followed by information on each of the variations observed:

```output
#CHROM  POS     ID      REF     ALT     QUAL    FILTER  INFO    FORMAT  results/bam/SRR2584866.aligned.sorted.bam
CP000819.1      1521    .       C       T       207     .       DP=9;VDB=0.993024;SGB=-0.662043;MQSB=0.974597;MQ0F=0;AC=1;AN=1;DP4=0,0,4,5;MQ=60
CP000819.1      1612    .       A       G       225     .       DP=13;VDB=0.52194;SGB=-0.676189;MQSB=0.950952;MQ0F=0;AC=1;AN=1;DP4=0,0,6,5;MQ=60
CP000819.1      9092    .       A       G       225     .       DP=14;VDB=0.717543;SGB=-0.670168;MQSB=0.916482;MQ0F=0;AC=1;AN=1;DP4=0,0,7,3;MQ=60
CP000819.1      9972    .       T       G       214     .       DP=10;VDB=0.022095;SGB=-0.670168;MQSB=1;MQ0F=0;AC=1;AN=1;DP4=0,0,2,8;MQ=60      GT:PL
CP000819.1      10563   .       G       A       225     .       DP=11;VDB=0.958658;SGB=-0.670168;MQSB=0.952347;MQ0F=0;AC=1;AN=1;DP4=0,0,5,5;MQ=60
CP000819.1      22257   .       C       T       127     .       DP=5;VDB=0.0765947;SGB=-0.590765;MQSB=1;MQ0F=0;AC=1;AN=1;DP4=0,0,2,3;MQ=60      GT:PL
CP000819.1      38971   .       A       G       225     .       DP=14;VDB=0.872139;SGB=-0.680642;MQSB=1;MQ0F=0;AC=1;AN=1;DP4=0,0,4,8;MQ=60      GT:PL
CP000819.1      42306   .       A       G       225     .       DP=15;VDB=0.969686;SGB=-0.686358;MQSB=1;MQ0F=0;AC=1;AN=1;DP4=0,0,5,9;MQ=60      GT:PL
CP000819.1      45277   .       A       G       225     .       DP=15;VDB=0.470998;SGB=-0.680642;MQSB=0.95494;MQ0F=0;AC=1;AN=1;DP4=0,0,7,5;MQ=60
CP000819.1      56613   .       C       G       183     .       DP=12;VDB=0.879703;SGB=-0.676189;MQSB=1;MQ0F=0;AC=1;AN=1;DP4=0,0,8,3;MQ=60      GT:PL
CP000819.1      62118   .       A       G       225     .       DP=19;VDB=0.414981;SGB=-0.691153;MQSB=0.906029;MQ0F=0;AC=1;AN=1;DP4=0,0,8,10;MQ=59
CP000819.1      64042   .       G       A       225     .       DP=18;VDB=0.451328;SGB=-0.689466;MQSB=1;MQ0F=0;AC=1;AN=1;DP4=0,0,7,9;MQ=60      GT:PL
```

This is a lot of information, so let's take some time to make sure we understand our output.

To make this information display more clearly, here's a better-formatted version of your tables and explanation:

### Predicted Variation Information

The first few columns represent the information we have about a predicted variation:

| Column  | Information                                                                                                                                                                 | 
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| CHROM   | Contig location where the variation occurs                                                                                                                                  | 
| POS     | Position within the contig where the variation occurs                                                                                                                       | 
| ID      | A `.` until annotation information is added                                                                                                                                  | 
| REF     | Reference genotype (forward strand)                                                                                                                                          | 
| ALT     | Sample genotype (forward strand)                                                                                                                                             | 
| QUAL    | Phred-scaled probability that the observed variant exists at this site (higher values indicate higher confidence)                                                            | 
| FILTER  | Indicates if a variant has passed quality filters: a `.` if no filters have been applied, "PASS" if it passed, or the name of the failed filters                             | 

In an ideal scenario, the information in the `QUAL` column would suffice to filter out low-quality variant calls. However, in practice, multiple other metrics are considered for accurate filtering.

### Genotype Information

The last two columns contain the genotypes, which can be tricky to decode:

| Column  | Information                                                                                                                                             | 
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------| 
| FORMAT  | Lists, in order, the metrics presented in the final column                                                                                               | 
| results | Contains the values associated with those metrics in the same order                                                                                      | 

For our file, the metrics presented are `GT:PL:GQ`.

### Metric Definitions

| Metric  | Definition                                                                                                                                                                      | 
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| AD, DP  | Depth per allele by sample and coverage                                                                                                                                        | 
| GT      | Genotype for the sample at this locus. For diploid organisms, the GT field indicates the two alleles carried by the sample: 0 for the REF allele, 1 for the first ALT allele, etc. A "0/0" indicates homozygous reference, "0/1" indicates heterozygous, and "1/1" indicates homozygous for the alternate allele. | 
| PL      | Likelihoods of the given genotypes                                                                                                                                             | 
| GQ      | Phred-scaled confidence for the genotype                                                                                                                                       | 

This format makes the tables more readable and provides clear headings and definitions for each column or metric.


The Broad Institute's [VCF guide](https://gatk.broadinstitute.org/hc/en-us/articles/360035531692-VCF-Variant-Call-Format) is an excellent place to learn more about the VCF file format.


---


### Exercise

Use the `grep` and `wc` commands you have learned to assess how many variants are in the vcf file.


Cheat answer: there are 766 variants in this file, but work that out for yourself!


---


### Assess the alignment (visualization)

It is often instructive to look at your data in a genome browser. Visualization will allow you to get a "feel" for
the data, as well as detecting abnormalities and problems. Also, exploring the data in such a way may give you
ideas for further analyses.  As such, visualization tools are useful for exploratory analysis. In this lesson we
will describe two different tools for visualization: a light-weight command-line based one and the Broad
Institute's Integrative Genomics Viewer (IGV) which requires
software installation and transfer of files.

In order for us to visualize the alignment files, we will need to index the BAM file using `samtools`:

```bash
$ samtools index results/bam/SRR2584866.aligned.sorted.bam
```

---


#### Viewing with `tview`

[Samtools](https://www.htslib.org/) implements a very simple text alignment viewer based on the GNU
`ncurses` library, called `tview`. This alignment viewer works with short indels and shows [MAQ](https://maq.sourceforge.net/) consensus.
It uses different colors to display mapping quality or base quality, subjected to users' choice. Samtools viewer is known to work with a 130 GB alignment swiftly. Due to its text interface, displaying alignments over network is also very fast.

In order to visualize our mapped reads, we use `tview`, giving it the sorted bam file and the reference file:

```bash
$ samtools tview results/bam/SRR2584866.aligned.sorted.bam data/ref_genome/ecoli_rel606.fasta
```

```output
1         11        21        31        41        51        61        71        81        91        101       111       121  
AGCTTTTCATTCTGACTGCAACGGGCAATATGTCTCTGTGTGGATTAAAAAAAGAGTGTCTGATAGCAGCTTCTGAACTGGTTACCTGCCGTGAGTAAATTAAAATTTTATTGACTTAGGTCACT
.............................................................................................................................
...............................................................................,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,                 ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
  ...........................................................................................................................
                                .............................................................................................
                                                                ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
                                                                  ...........................................................
                                                                                            .................................
                                                                                                           ,,,,,,,,,,,,,,,,,,
                                                                                                                .............
                                                                                                                           ..

```


 ---

 
The first line of output shows the genome coordinates in our reference genome. 
The second line shows the reference genome sequence. 
The third line shows the consensus sequence determined from the sequence reads. 

A `.` indicates a match to the reference sequence, so we can see that the consensus from our sample matches the reference in most locations. That is good! If that was not the case, we should probably reconsider our choice of reference. 
- Commas denote matching bases on the reverse strand, while dots represent matching bases on the forward strand.

Below the horizontal line, we can see all of the reads in our sample aligned with the reference genome. 
- Only positions where the called base (ATGCN) differs from the reference are shown.
- Uppercase letters represent mismatched bases from the sequencing reads that are aligned to the forward strand.
- Lowercase Letters (atcgn) represent mismatched bases from the sequencing reads that are aligned to the reverse strand.

You can use the arrow keys on your keyboard to scroll or type `?` for a help menu. 

To navigate to a specific position, type `g`. A dialogue box will appear. 
- In this box, type the name of the "chromosome" followed by a colon and the position of the variant you would like to view (e.g. for this sample, type `CP000819.1:50` to view the 50th base. Type `Ctrl^C` or `q` to exit `tview`.

This output helps visualize coverage and variations at different positions in the genome, showing which areas have high confidence matches and where mismatches or gaps occur.

---


### Exercise

Visualize the alignment of the reads for our `SRR2584866` sample. What variant is present at
position 4377265? What is the canonical nucleotide in that position?   

### Solution

```bash
$ samtools tview ~/dc_workshop/results/bam/SRR2584866.aligned.sorted.bam ~/dc_workshop/data/ref_genome/ecoli_rel606.fasta
```

Then type `g`. In the dialogue box, type `CP000819.1:4377265`.
`G` is the variant. `A` is canonical. This variant possibly changes the phenotype of this sample to hypermutable. It occurs in the gene *mutL*, which controls DNA mismatch repair.

---

## Bonus exercise if time and energy and skill permit!

if you are realy keen, work through the final lesson from the Data Carprentry course: [https://datacarpentry.org/wrangling-genomics/05-automation.html](https://datacarpentry.org/wrangling-genomics/05-automation.html)

But note that you will probably have to adapt some of the commands to ensure they work okay on your Jupyter notebook server, as the Data Carprentry course is set up to run on Amazon Web Services (AWS) instances. Not for the faint-hearted.


---


# That's enough for this week!
Please fill in the [feedback form](https://forms.office.com/Pages/ResponsePage.aspx?id=1O9S5qxdtkGiPXZGNMNs6nlK4JpdWo9LrIwG-Ilh-z9UMDEzSUNWNUxOTE1MTEZIQzg1T0oySkhRVS4u&origin=Invitation&channel=0) before leaving this session. 

As a reward for all your hard work, you should now take a look at this rap video that features Richard Lenski's research group along with Mark Pallen and his colleagues when he worked at Warwick. Have a great weekend!

<iframe width="560" height="315" src="https://www.youtube.com/embed/swoDROzsjXg?si=5n_GWma2mJFPA-fH" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---


