---
title: Friday morning — Finding sequence variants
---

# Finding sequence variants

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
```   ### Filtering

The `vcfutils.pl varFilter` call filters out variants that do not meet minimum quality default criteria, which can be changed through
its options. Using `bcftools` we can verify that the quality of the variant call set has improved after this filtering step by
calculating the ratio of [transitions(TS)](https://en.wikipedia.org/wiki/Transition_%28genetics%29) to
[transversions (TV)](https://en.wikipedia.org/wiki/Transversion) ratio (TS/TV),
where transitions should be more likely to occur than transversions:

```bash
$ bcftools stats results/vcf/SRR2584866_variants.vcf | grep TSTV
# TSTV, transitions/transversions:
# TSTV	[2]id	[3]ts	[4]tv	[5]ts/tv	[6]ts (1st ALT)	[7]tv (1st ALT)	[8]ts/tv (1st ALT)
TSTV	0	628	58	10.83	628	58	10.83
$ bcftools stats results/vcf/SRR2584866_final_variants.vcf | grep TSTV
# TSTV, transitions/transversions:
# TSTV	[2]id	[3]ts	[4]tv	[5]ts/tv	[6]ts (1st ALT)	[7]tv (1st ALT)	[8]ts/tv (1st ALT)
TSTV	0	621	54	11.50	621	54	11.50
```   ### Explore the VCF format:

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

The first few columns represent the information we have about a predicted variation.

| column  | info                                                                                                                                                                                                                                                                                                                                    | 
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CHROM   | contig location where the variation occurs                                                                                                                                                                                                                                                                                              | 
| POS     | position within the contig where the variation occurs                                                                                                                                                                                                                                                                                   | 
| ID      | a `.` until we add annotation information                                                                                                                                                                                                                                                                                                                                      | 
| REF     | reference genotype (forward strand)                                                                                                                                                                                                                                                                                                     | 
| ALT     | sample genotype (forward strand)                                                                                                                                                                                                                                                                                                        | 
| QUAL    | Phred-scaled probability that the observed variant exists at this site (higher is better)                                                                                                                                                                                                                                               | 
| FILTER  | a `.` if no quality filters have been applied, PASS if a filter is passed, or the name of the filters this variant failed                                                                                                                                                                                                                                                                                                                                      | 

In an ideal world, the information in the `QUAL` column would be all we needed to filter out bad variant calls.
However, in reality we need to filter on multiple other metrics.

The last two columns contain the genotypes and can be tricky to decode.

| column  | info                                                                                                                                                                                                                                                                                                                                    | 
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FORMAT  | lists in order the metrics presented in the final column                                                                                                                                                                                                                                                                                | 
| results | lists the values associated with those metrics in order                                                                                                                                                                                                                                                                                 | 

For our file, the metrics presented are GT:PL:GQ.

| metric  | definition                                                                                                                                                                                                                                                                                                                              | 
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AD, DP  | the depth per allele by sample and coverage                                                                                                                                                                                                                                                                                             | 
| GT      | the genotype for the sample at this loci. For a diploid organism, the GT field indicates the two alleles carried by the sample, encoded by a 0 for the REF allele, 1 for the first ALT allele, 2 for the second ALT allele, etc. A 0/0 means homozygous reference, 0/1 is heterozygous, and 1/1 is homozygous for the alternate allele. | 
| PL      | the likelihoods of the given genotypes                                                                                                                                                                                                                                                                                                  | 
| GQ      | the Phred-scaled confidence for the genotype                                                                                                                                                                                                                                                                                            | 

The Broad Institute's [VCF guide](https://www.broadinstitute.org/gatk/guide/article?id=1268) is an excellent place
to learn more about the VCF file format.


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
AGCTTTTCATTCTGACTGCAACGGGCAATATGTCTCTGTGTGGATTAAAAAAAGAGTGTCTGATAGCAGCTTCTGAACTGGTTACCTGCCGTGAGTAAATTAAAATTTTATTGACTTAGGTCACTAAATAC
..................................................................................................................................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,, ..................N................. ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,........................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,, ..................N................. ,,,,,,,,,,,,,,,,,,,,,,,,,,,.............................
...................................,g,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,  ....................................   ................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,....................................   ....................................      ,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,  ....................................  ,,a,,,,,,,,,,,,,,,,,,,,,,,,,,,,,     .......
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,, .............................  ,,,,,,,,,,,,,,,,,g,,,,,    ,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,  ...........................T.......   ,,,,,,,,,,,,,,,,,,,,,,,c,          ......
......................... ................................   ,g,,,,,,,,,,,,,,,,,,,      ...........................
,,,,,,,,,,,,,,,,,,,,, ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,, ,,,,,,,,,,,,,,,,,,,,,,,,,,,       ..........................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,   ................................T..  ..............................   ,,,,,,
...........................       ,,,,,,g,,,,,,,,,,,,,,,,,   ....................................         ,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,, ....................................  ...................................        ....
....................................  ........................  ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,      ....
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,  ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
........................            .................................. .............................     ....
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,   ....................................        ..........................
...............................       ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,, ....................................
...................................  ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,  ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,, ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,  ..................................
.................................... ,,,,,,,,,,,,,,,,,,a,,,,,,,,,,,,,,,,,        ,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,  ............................ ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
```

The first line of output shows the genome coordinates in our reference genome. The second line shows the reference
genome sequence. The third line shows the consensus sequence determined from the sequence reads. A `.` indicates
a match to the reference sequence, so we can see that the consensus from our sample matches the reference in most
locations. That is good! If that was not the case, we should probably reconsider our choice of reference.

Below the horizontal line, we can see all of the reads in our sample aligned with the reference genome. Only
positions where the called base differs from the reference are shown. You can use the arrow keys on your keyboard
to scroll or type `?` for a help menu. To navigate to a specific position, type `g`. A dialogue box will appear. In
this box, type the name of the "chromosome" followed by a colon and the position of the variant you would like to view
(e.g. for this sample, type `CP000819.1:50` to view the 50th base. Type `Ctrl^C` or `q` to exit `tview`.


---


### Exercise

Visualize the alignment of the reads for our `SRR2584866` sample. What variant is present at
position 4377265? What is the canonical nucleotide in that position?   ### Solution

```bash
$ samtools tview ~/dc_workshop/results/bam/SRR2584866.aligned.sorted.bam ~/dc_workshop/data/ref_genome/ecoli_rel606.fasta
```

Then type `g`. In the dialogue box, type `CP000819.1:4377265`.
`G` is the variant. `A` is canonical. This variant possibly changes the phenotype of this sample to hypermutable. It occurs
in the gene *mutL*, which controls DNA mismatch repair.   # Automating a Variant Calling Workflow


---

# Bonus exercise if time ane energy and skill permit!

## What is a shell script?

You wrote a simple shell script in a previous lesson that we used to extract bad reads from our FASTQ files and put them into a new file.

Here is the script you wrote:

```bash
grep -B1 -A2 NNNNNNNNNN *.fastq > scripted_bad_reads.txt

echo "Script finished!"
```

That script was only two lines long, but shell scripts can be much more complicated
than that and can be used to perform a large number of operations on one or many
files. This saves you the effort of having to type each of those commands over for
each of your data files and makes your work less error-prone and more reproducible.
For example, the variant calling workflow we just carried out had about eight steps
where we had to type a command into our terminal. Most of these commands were pretty
long. If we wanted to do this for all six of our data files, that would be forty-eight
steps. If we had 50 samples (a more realistic number), it would be 400 steps! You can
see why we want to automate this.

We have also used `for` loops in previous lessons to iterate one or two commands over multiple input files.
In these `for` loops, the filename was defined as a variable in the `for` statement, which enabled you to run the loop on multiple files. We will be using variable assignments like this in our new shell scripts.

Here is the `for` loop you wrote for unzipping `.zip` files:

```bash
$ for filename in *.zip
> do
> unzip $filename
> done
```

And here is the one you wrote for running Trimmomatic on all of our `.fastq` sample files:

```bash
$ for infile in *_1.fastq.gz
> do
>   base=$(basename ${infile} _1.fastq.gz)
>   trimmomatic PE ${infile} ${base}_2.fastq.gz \
>                ${base}_1.trim.fastq.gz ${base}_1un.trim.fastq.gz \
>                ${base}_2.trim.fastq.gz ${base}_2un.trim.fastq.gz \
>                SLIDINGWINDOW:4:20 MINLEN:25 ILLUMINACLIP:NexteraPE-PE.fa:2:40:15 
> done
```

Notice that in this `for` loop, we used two variables, `infile`, which was defined in the `for` statement, and `base`, which was created from the filename during each iteration of the loop.   ### Creating variables

Within the Bash shell you can create variables at any time (as we did
above, and during the 'for' loop lesson). Assign any name and the
value using the assignment operator: '='. You can check the current
definition of your variable by typing into your script: echo $variable\_name.   In this lesson, we will use two shell scripts to automate the variant calling analysis: one for FastQC analysis (including creating our summary file), and a second for the remaining variant calling. To write a script to run our FastQC analysis, we will take each of the commands we entered to run FastQC and process the output files and put them into a single file with a `.sh` extension. The `.sh` is not essential, but serves as a reminder to ourselves and to the computer that this is a shell script.

## Analyzing quality with FastQC

We will use the command `touch` to create a new file where we will write our shell script. We will create this script in a new
directory called `scripts/`. Previously, we used
`nano` to create and open a new file. The command `touch` allows us to create a new file without opening that file.

```bash
$ mkdir -p ~/dc_workshop/scripts
$ cd ~/dc_workshop/scripts
$ touch read_qc.sh
$ ls 
```

```output
read_qc.sh
```

We now have an empty file called `read_qc.sh` in our `scripts/` directory. We will now open this file in `nano` and start
building our script.

```bash
$ nano read_qc.sh
```

**Enter the following pieces of code into your shell script (not into your terminal prompt).**

Our first line will ensure that our script will exit if an error occurs, and is a good idea to include at the beginning of your scripts. The second line will move us into the `untrimmed_fastq/` directory when we run our script.

```output
set -e
cd ~/dc_workshop/data/untrimmed_fastq/
```

These next two lines will give us a status message to tell us that we are currently running FastQC, then will run FastQC
on all of the files in our current directory with a `.fastq` extension.

```output
echo "Running FastQC ..."
fastqc *.fastq*
```

Our next line will create a new directory to hold our FastQC output files. Here we are using the `-p` option for `mkdir` again. It is a good idea to use this option in your shell scripts to avoid running into errors if you do not have the directory structure you think you do.

```output
mkdir -p ~/dc_workshop/results/fastqc_untrimmed_reads
```

Our next three lines first give us a status message to tell us we are saving the results from FastQC, then moves all of the files
with a `.zip` or a `.html` extension to the directory we just created for storing our FastQC results.

```output
echo "Saving FastQC results..."
mv *.zip ~/dc_workshop/results/fastqc_untrimmed_reads/
mv *.html ~/dc_workshop/results/fastqc_untrimmed_reads/
```

The next line moves us to the results directory where we have stored our output.

```output
cd ~/dc_workshop/results/fastqc_untrimmed_reads/
```

The next five lines should look very familiar. First we give ourselves a status message to tell us that we are unzipping our ZIP
files. Then we run our for loop to unzip all of the `.zip` files in this directory.

```output
echo "Unzipping..."
for filename in *.zip
    do
    unzip $filename
    done
```

Next we concatenate all of our summary files into a single output file, with a status message to remind ourselves that this is
what we are doing.

```output
echo "Saving summary..."
mkdir -p ~/dc_workshops/docs
cat */summary.txt > ~/dc_workshop/docs/fastqc_summaries.txt
```   ### Using `echo` statements

We have used `echo` statements to add progress statements to our script. Our script will print these statements
as it is running and therefore we will be able to see how far our script has progressed.   Your full shell script should now look like this:

```output
set -e
cd ~/dc_workshop/data/untrimmed_fastq/

echo "Running FastQC ..."
fastqc *.fastq*

mkdir -p ~/dc_workshop/results/fastqc_untrimmed_reads

echo "Saving FastQC results..."
mv *.zip ~/dc_workshop/results/fastqc_untrimmed_reads/
mv *.html ~/dc_workshop/results/fastqc_untrimmed_reads/

cd ~/dc_workshop/results/fastqc_untrimmed_reads/

echo "Unzipping..."
for filename in *.zip
    do
    unzip $filename
    done

echo "Saving summary..."
cat */summary.txt > ~/dc_workshop/docs/fastqc_summaries.txt
```

Save your file and exit `nano`. We can now run our script:

```bash
$ bash read_qc.sh
```

```output
Running FastQC ...
Started analysis of SRR2584866.fastq
Approx 5% complete for SRR2584866.fastq
Approx 10% complete for SRR2584866.fastq
Approx 15% complete for SRR2584866.fastq
Approx 20% complete for SRR2584866.fastq
Approx 25% complete for SRR2584866.fastq
. 
. 
. 
```

For each of your sample files, FastQC will ask if you want to replace the existing version with a new version. This is
because we have already run FastQC on this samples files and generated all of the outputs. We are now doing this again using
our scripts. Go ahead and select `A` each time this message appears. It will appear once per sample file (six times total).

```output
replace SRR2584866_fastqc/Icons/fastqc_icon.png? [y]es, [n]o, [A]ll, [N]one, [r]ename:
```

## Automating the rest of our variant calling workflow

We can extend these principles to the entire variant calling workflow. To do this, we will take all of the individual commands that we wrote before, put them into a single file, add variables so that the script knows to iterate through our input files and write to the appropriate output files. This is very similar to what we did with our `read_qc.sh` script, but will be a bit more complex.

Download the script from [here](files/run_variant_calling.sh). Download to `~/dc_workshop/scripts`.

```bash
curl -O https://datacarpentry.org/wrangling-genomics/files/run_variant_calling.sh
```

Our variant calling workflow has the following steps:

1. Index the reference genome for use by bwa and samtools.
2. Align reads to reference genome.
3. Convert the format of the alignment to sorted BAM, with some intermediate steps.
4. Calculate the read coverage of positions in the genome.
5. Detect the single nucleotide variants (SNVs).
6. Filter and report the SNVs in VCF (variant calling format).

Let's go through this script together:

```bash
$ cd ~/dc_workshop/scripts
$ less run_variant_calling.sh
```

The script should look like this:

```output
set -e
cd ~/dc_workshop/results

genome=~/dc_workshop/data/ref_genome/ecoli_rel606.fasta

bwa index $genome

mkdir -p sam bam bcf vcf

for fq1 in ~/dc_workshop/data/trimmed_fastq_small/*_1.trim.sub.fastq
    do
    echo "working with file $fq1"

    base=$(basename $fq1 _1.trim.sub.fastq)
    echo "base name is $base"

    fq1=~/dc_workshop/data/trimmed_fastq_small/${base}_1.trim.sub.fastq
    fq2=~/dc_workshop/data/trimmed_fastq_small/${base}_2.trim.sub.fastq
    sam=~/dc_workshop/results/sam/${base}.aligned.sam
    bam=~/dc_workshop/results/bam/${base}.aligned.bam
    sorted_bam=~/dc_workshop/results/bam/${base}.aligned.sorted.bam
    raw_bcf=~/dc_workshop/results/bcf/${base}_raw.bcf
    variants=~/dc_workshop/results/vcf/${base}_variants.vcf
    final_variants=~/dc_workshop/results/vcf/${base}_final_variants.vcf 

    bwa mem $genome $fq1 $fq2 > $sam
    samtools view -S -b $sam > $bam
    samtools sort -o $sorted_bam $bam
    samtools index $sorted_bam
    bcftools mpileup -O b -o $raw_bcf -f $genome $sorted_bam
    bcftools call --ploidy 1 -m -v -o $variants $raw_bcf 
    vcfutils.pl varFilter $variants > $final_variants
   
    done
```

Now, we will go through each line in the script before running it.

First, notice that we change our working directory so that we can create new results subdirectories
in the right location.

```output
cd ~/dc_workshop/results
```

Next we tell our script where to find the reference genome by assigning the `genome` variable to
the path to our reference genome:

```output
genome=~/dc_workshop/data/ref_genome/ecoli_rel606.fasta
```

Next we index our reference genome for BWA:

```output
bwa index $genome
```

And create the directory structure to store our results in:

```output
mkdir -p sam bam bcf vcf
```

Then, we use a loop to run the variant calling workflow on each of our FASTQ files. The full list of commands
within the loop will be executed once for each of the FASTQ files in the
`data/trimmed_fastq_small/` directory.
We will include a few `echo` statements to give us status updates on our progress.

The first thing we do is assign the name of the FASTQ file we are currently working with to a variable called `fq1` and
tell the script to `echo` the filename back to us so we can check which file we are on.

```bash
for fq1 in ~/dc_workshop/data/trimmed_fastq_small/*_1.trim.sub.fastq
    do
    echo "working with file $fq1"
```

We then extract the base name of the file (excluding the path and `.fastq` extension) and assign it
to a new variable called `base`.

```bash
    base=$(basename $fq1 _1.trim.sub.fastq)
    echo "base name is $base"
```

We can use the `base` variable to access both the `base_1.fastq` and `base_2.fastq` input files, and create variables to store the names of our output files. This makes the script easier to read because we do not need to type out the full name of each of the files: instead, we use the `base` variable, but add a different extension (e.g. `.sam`, `.bam`) for each file produced by our workflow.

```bash
    #input fastq files
    fq1=~/dc_workshop/data/trimmed_fastq_small/${base}_1.trim.sub.fastq
    fq2=~/dc_workshop/data/trimmed_fastq_small/${base}_2.trim.sub.fastq
    
    # output files
    sam=~/dc_workshop/results/sam/${base}.aligned.sam
    bam=~/dc_workshop/results/bam/${base}.aligned.bam
    sorted_bam=~/dc_workshop/results/bam/${base}.aligned.sorted.bam
    raw_bcf=~/dc_workshop/results/bcf/${base}_raw.bcf
    variants=~/dc_workshop/results/bcf/${base}_variants.vcf
    final_variants=~/dc_workshop/results/vcf/${base}_final_variants.vcf     
```

And finally, the actual workflow steps:

1) align the reads to the reference genome and output a `.sam` file:

```output
    bwa mem $genome $fq1 $fq2 > $sam
```

2) convert the SAM file to BAM format:

```output
    samtools view -S -b $sam > $bam
```

3) sort the BAM file:

```output
    samtools sort -o $sorted_bam $bam 
```

4) index the BAM file for display purposes:

```output
    samtools index $sorted_bam
```

5) calculate the read coverage of positions in the genome:

```output
    bcftools mpileup -O b -o $raw_bcf -f $genome $sorted_bam 
```

6) call SNVs with bcftools:

```output
    bcftools call --ploidy 1 -m -v -o $variants $raw_bcf 
```

7) filter and report the SNVs in variant calling format (VCF):

```output
    vcfutils.pl varFilter $variants  > $final_variants
```   ### Exercise

It is a good idea to add comments to your code so that you (or a collaborator) can make sense of what you did later.
Look through your existing script. Discuss with a neighbor where you should add comments. Add comments (anything following
a `#` character will be interpreted as a comment, bash will not try to run these comments as code).   Now we can run our script:

```bash
$ bash run_variant_calling.sh
```   ### Exercise

The samples we just performed variant calling on are part of the long-term evolution experiment introduced at the
beginning of our variant calling workflow. From the metadata table, we know that SRR2589044 was from generation 5000,
SRR2584863 was from generation 15000, and SRR2584866 was from generation 50000. How did the number of mutations per sample change
over time? Examine the metadata table. What is one reason the number of mutations may have changed the way they did?

Hint: You can find a copy of the output files for the subsampled trimmed FASTQ file variant calling in the
`~/.solutions/wrangling-solutions/variant_calling_auto/` directory.   ### Solution

```bash
$ for infile in ~/dc_workshop/results/vcf/*_final_variants.vcf
> do
>     echo ${infile}
>     grep -v "#" ${infile} | wc -l
> done
```

For SRR2589044 from generation 5000 there were 10 mutations, for SRR2584863 from generation 15000 there were 25 mutations,
and SRR2584866 from generation 50000 there were 766 mutations. In the last generation, a hypermutable phenotype had evolved, causing this
strain to have more mutations.      ### Bonus exercise

If you have time after completing the previous exercise, use `run_variant_calling.sh` to run the variant calling pipeline
on the full-sized trimmed FASTQ files. You should have a copy of these already in `~/dc_workshop/data/trimmed_fastq`, but if
you do not, there is a copy in `~/.solutions/wrangling-solutions/trimmed_fastq`. Does the number of variants change per sample?   ### Keypoints

- We can combine multiple commands into a shell script to automate a workflow.
- Use `echo` statements within your scripts to get an automated progress update.   
---

## That's enough for this week!
As a reward for all your hard work, you should now take a look at this rap video that features Richard Lenski's research group along with Mark Pallen and his colleagues when he worked at Warwick. Have a great weekend!

<iframe width="560" height="315" src="https://www.youtube.com/embed/swoDROzsjXg?si=5n_GWma2mJFPA-fH" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
