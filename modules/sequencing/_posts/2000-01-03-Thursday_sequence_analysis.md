---
title: Analyses of YOUR VERY OWN short and long reads!

---


# Week 2: Thursday 24th October 
**Venue:** QIB Board Room 9.30am
**Tutor** Mark Pallen

---

**Reminder**: Please fill in the [feedback form](https://forms.office.com/e/si0HtJNKD7) before you leave on friday!

---
# Be excited!

Today you are going to analyse the sequence reads you created earlier this week. 

BUT to start with, we will have a walkthrough of the various steps in the analysis, working on sequences created by students last year. Then you will work in two or three groups to analyse all the sequences that you created for yourselves. 

 - Some of these steps will involve programs you used last week so will be covered very quickly. 
 - Some will involve new programs, where you will be provided with more background information.


The steps in the workflow ahead of us include using:

 - `FASTQC` and `Trimmomatic` to evaluate and improve the quality of short reads
 - `Nanoplot` and `PoreChop` to evaluate and improve the quality of short reads
 - `Kraken2`, `Krona` and `blast` for taxonomic profiling of reads 
 - `Spades` to assemble short reads
 - `Flye` to assemble long reads
 - a custom Python script to evaluate assemblies
 - as stretch target, `Unicycler` to perform hybrid assembly. 
 
---
 
# The walkthrough
![](https://raw.githubusercontent.com/mmbdtp/mmbdtp.github.io/gh-pages/modules/sequencing/_posts/2ff0bd4a-85a5-46a4-902c-73106a3b0717.webp)

---
## Getting the example sequences

Open a Terminal on your Notebook server. 

Sometimes Conda can run into issues with outdated metadata. It might make life easier if you clear the Conda cache and tell Conda what channels to look at as default

```
conda clean --all
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict ⁠
```

Let's get started by setting up a specific conda environment for this session and installing all the software we are going to need. If we include names of software packages after the environment name, everything gets done in one go and this usually works better than installing the packges one at a time.
```
conda create -y -q -n seqanalysis fastqc trimmomatic kraken2 krona NanoPlot porechop seqtk flye
conda activate seqanalysis
```

Let's create and use a directory for these example analyses:

```
mkdir seqanalysis_example
cd seqanalysis_example
```

One of the key features of the Jupyter Notebook servers run by [CLIMB-BIG-DATA](https://climb.ac.uk) is access to shared directories. There are two of these `shared-team` and `shared-public`. 
You can take a look at them using `ls` or via the graphical file browser.

```
ls ../shared-public
```

```
ls ../shared-team
```


The `shared-public` directory is used to share key resources that lots of people want to use but which are tricky to download or install, including large libraries. 

The `shared-team` directory is used for material that one wants to share with your whole team. When you are using CLIMB-BIG_DATA for your own research, this means your research group. But in this case, we are using it to access training material and analyses carried out by the MMB DTP cohorts as part of their bioinformatics training.

Let's grab two of the sequences created by last year's cohort. 

We can grab FASTQ files from a sample containing short reads like this:

```
cp /home/jovyan/shared-team/year1_training/week2/sequence-data/DTP-2-3_S11_L001_R*_001.fastq.gz .
```


We can grab a FASTQ file containing long reads here

```
cp /home/jovyan/shared-team/year1_training/week2/long_reads/DTP_1_1_Nano.fastq.gz .
```

Let's tidy things up by keeping short and long reads in different directories.

```
mkdir short_reads
mv *R* short_reads
mkdir long_reads 
mv *Nano* long_reads
```

---

## QC on short reads

---

Let's run trimmomatic on the short reads. 
Move into the `short_reads` directory

```
cd short_reads
```

As before, let's copy across the adapters file

```
cp /home/jovyan/shared-team/conda/mpallen.mmb-dtp/seqanalysis/share/trimmomatic/adapters/NexteraPE-PE.fa .
```

We are using my version of the adapters file, which is associated with my version of the `trimmomatic` package. Feel free to explore `/home/jovyan/shared-team/conda/` followed by your name to see your own directories there.

Now run `trimmomatic`

```
trimmomatic PE -threads 4 DTP-2-3_S11_L001_R1_001.fastq.gz DTP-2-3_S11_L001_R2_001.fastq.gz     DTP-2-3_S11_L001_R1_paired.fastq.gz DTP-2-3_S11_L001_R1_unpaired.fastq.gz     DTP-2-3_S11_L001_R2_paired.fastq.gz DTP-2-3_S11_L001_R2_unpaired.fastq.gz     ILLUMINACLIP:NexteraPE-PE.fa:2:30:10 SLIDINGWINDOW:4:20 MINLEN:25
```

And then let's run fastqc over all the fastq files.

```
fastqc *fastq*
```
then look at what has happened to file sizes

```
ls -alSh
```

And if you work through the html files, you can see that the sequences were not too bad to start with, but got a bit better on trimming.

You will be doing this on all the files you created yourselves later today.

---

## Taxonomic profiling of short reads 

---

If we want to learn quickly what organisms are represented by sequences in a run, we can use various approaches to taxonomic profiling. We will now have a brief [Powerpoint presentation](https://raw.githubusercontent.com/mmbdtp/mmbdtp.github.io/gh-pages/modules/sequencing/_posts/Sequence_profiling.pptx) on this topic.

Let's engage in some housekeeping by making directories for the QC we just did and for the `kraken` analyses. We will use `kraken` on the trimmed reads

```
mkdir QC kraken
cp *_paired.fastq.gz kraken
mv *.* QC
```
---
### Kraken2 databases 

---

Pre-computed Kraken2 databases are available on the `/shared-public` file system within CLIMB-BIG-DATA. These databases are downloaded from Ben Langmead's publicly available [Kraken2 indexes page](https://benlangmead.github.io/aws-indexes/k2). These datasets are updated monthly and we will keep the latest versions available.

The `/shared-public` area is designed to store frequently used, important databases for the microbial genomics community. We are just getting started building this resource so please contact us with suggestions for other databases you would like to see here.

We can take a look at the databases that are available and their sizes:

```
du -h -d1 /home/jovyan//shared-public/db/kraken2

16G     /shared-public/db/kraken2/pluspfp_8gb
152G    /shared-public/db/kraken2/pluspf
16G     /shared-public/db/kraken2/standard_8gb
743G    /shared-public/db/kraken2/nt
341G    /shared-public/db/kraken2/pluspfp
30G     /shared-public/db/kraken2/pluspf_16gb
31G     /shared-public/db/kraken2/pluspfp_16gb
16G     /shared-public/db/kraken2/pluspf_8gb
142G    /shared-public/db/kraken2/standard
11G     /shared-public/db/kraken2/eupathDB46
20G     /shared-public/db/kraken2/minusb
30G     /shared-public/db/kraken2/standard_16gb
1.3G    /shared-public/db/kraken2/viral
1.6T    /shared-public/db/kraken2

```

 - The `du` command in Unix/Linux systems stands for "disk usage." It is used to estimate the space used by files and directories. The options you provided, -h and -d1, modify its behavior as follows:

   - -h (human-readable): This option makes the output easier for humans to read by displaying the sizes in kilobytes (K), megabytes (M), or gigabytes (G), instead of raw byte counts.

   - -d1 (depth 1): This option restricts the depth of directories shown to 1 level. It means that it will only show the sizes of the top-level directories and files within the specified directory, without descending into subdirectories deeper than 1 level.

We can run `kraken2` directly within this JupyterHub notebook which is running in a container. A standard container has 8 CPU cores and 64GB of memory. `kraken2` doesn't run well unless the database fits into memory, so we can use one of the smaller databases for now such as k2_standard_8gb which contains archaea, bacteria, viral, plasmid, human and UniVec_Core sequences from RefSeq, but subsampled down to a 8Gb database. This will be fast, but we trade off specificity and sensitivity against bigger databases.

```
cd kraken
kraken2 --threads 8 --db /home/jovyan/shared-public/db/kraken2/standard_8gb/latest --output DTP-2-3_reads_hits.txt --report DTP-2-3_reads_report.txt  --use-names  DTP-2-3_S11_L001_R1_paired.fastq.gz DTP-2-3_S11_L001_R2_paired.fastq.gz
```


The `DTP-2-3_reads_report.txt` file gives a supposedly human-readable output from Kraken2.

```
cat DTP-2-3_reads_report.txt  
```

But most people would agree it is not all that human-readable! 

 - Download it and open with Excel. But even then it isn't great.

The output from Kraken 2  contains several columns:

 - *Percentage*: This column shows the percentage of reads (sequences) that have been classified at or below a particular taxonomic level.
 - *Number of reads* (NumReads): This column gives the raw count of reads that have been assigned to the taxon or any of its descendants.
 - *Number of distinct reads* (NumDistinctReads): This column may be present in some versions and shows the unique number of reads assigned to the taxon (not duplicated).
 - *Taxonomic ID* (TaxID): This is a unique identifier assigned to each taxon by the NCBI Taxonomy Database.
 - *Rank code*: This is a single letter indicating the taxonomic rank (for example, 'D' for domain, 'P' for phylum, 'G' for genus, 'S' for species).
 - *Scientific name*: This column lists the scientific name of the organism or taxon to which the reads have been classified.


An alternative is to load a graphical display of the kraken results. 

To do that we need to use `krona`, which needs us to run `ktUpdateTaxonomy.sh` before it can be used.

```
ktUpdateTaxonomy.sh
```

Now let's run `krona`

```
ktImportTaxonomy -t 5 -m 3 DTP-2-3_reads_report.txt   -o DTP-2-3_reads_KronaReport.html
```

Take a look in the graphical file viewer and you will see a file called `DTP-2-3_reads_KronaReport.html`

Click on it and it will open a window. Click on the "Trust HTML" option at the top left. Then navigate up and down the `krona` plot. 

Navigating a `krona` display is quite intuitive:

 - *Zooming In and Out*: To explore deeper taxonomic levels (e.g., from phylum to genus), you can click on any segment in the Krona chart. This action will "zoom in" to show more specific classifications within that category. To zoom back out, click on the center or use the back button in your browser.
 - *Hover for Details*: When you hover over a segment, you'll see detailed information, such as the name of the taxon and the percentage of reads or data that fall into that category. This helps you quickly get more information without clicking.
 - *Color-Coded Segments*: The segments are color-coded to differentiate between different taxonomic levels or categories. The colors can help you visually distinguish between groups as you explore the data.
 - *Search*: You can use the search bar (if available) to quickly locate specific taxa within the display, which is useful when you’re looking for particular organisms or classifications.
 - *Overall*, Krona allows you to interactively explore complex, hierarchical data in an easy-to-navigate, circular layout.

What do we conclude about the taxonomic profile of the organisms that went into this sample?

---

### A quick BLAST

---


Let's try another way of finding out what is in the file using the program `blast`. 

 - BLAST (Basic Local Alignment Search Tool) is a widely used bioinformatics tool for comparing nucleotide or protein sequences to sequence databases. It identifies regions of similarity between sequences, helping researchers determine evolutionary relationships, functional annotations, or identify unknown sequences. BLAST works by aligning query sequences with those in a database, providing a score based on the degree of similarity, and highlighting areas of significant match. It’s a powerful tool for tasks such as gene discovery, species identification, and comparative genomics. BLAST variants (like BLASTn, BLASTp) are optimized for different types of sequence queries.


Although we could install and interact with this very powerful program over the command line, it is tricky doing searches of the entire `nt` database this way, so we will use the web interface at NCBI instead. 

But we cannot throw hundreds of thousands of reads at NCBI Blast. So, first let's extract the first hundred sequences from our fastq file and convert them into fasta format using a program called `seqtk`.

```
mkdir ../blast
cp DTP-2-3_S11_L001_R1_paired.fastq.gz ../blast
cd ../blast
gunzip *gz
head -400 DTP-2-3_S11_L001_R1_paired.fastq > seqhead.fastq
seqtk seq -a seqhead.fastq > seqhead.fasta
```

Now `cat` the contents of seqhead.fasta or open with the file viewer. 

Copy the contents and paste into the [blastn web page](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastn&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome)

 - In the Organism box type 'Bacteria' and when it autofills, select 'Bacteria (taxid:2)'. 

 - Select the 'Somewhat Similar Sequences' option.
 - Tick the show results in new window option

 - And wait patiently for the results.

What do you see? What do you conclude? What is good and bad about this approach?

---

### A quick assembly

---

Let's assemble the short reads using `SPAdes`.

```
mkdir /home/jovyan/seqanalysis_example/short_reads/spades
cd /home/jovyan/seqanalysis_example/short_reads/spades
cp /home/jovyan/seqanalysis_example/short_reads/QC/DTP-2-3_S11_L001_R*_paired.fastq.gz . 
spades.py -1 DTP-2-3_S11_L001_R1_paired.fastq.gz -2 DTP-2-3_S11_L001_R2_paired.fastq.gz -o spades_out --threads 8 --memory 16
```

`SPAdes` (St. Petersburg genome assembler) is a popular tool used for assembling genomes, particularly for microbial and metagenomic datasets. When running SPAdes, the output folder contains several important files and directories that might want to familiarise yourselves with. Key files include:

1. **`contigs.fasta`**: This is one of the most important output files, containing the assembled contigs (continuous sequences). These are the primary assembled pieces of the genome.
2. **`scaffolds.fasta`**: Scaffolds are contigs that have been connected based on paired-end reads, though they may contain gaps.
3. **`K-mer directories`**: These subfolders (e.g., `K21`, `K33`) contain intermediate assembly files generated at different k-mer sizes during the assembly process, which is part of SPAdes’ multi-kmer approach to improve assembly accuracy.
4. **`assembly_graph.fastg`**: This file represents the assembly graph, which shows the relationship between different contigs and is useful for understanding the assembly process, especially in complex regions of the genome.
5. **`misc` folder**: Contains log files and other data that help diagnose issues or track the parameters used during the assembly.


How can we judge the quality of the assembly quickly and easily?

Let's just grab headers for contigs

```
grep ">" spades_out/contigs.fasta
```

 - What do you make of the output?

There are programs like Quast that can give a more sophisticated view of assembly stats, but that wouldn't install for me earlier in the so with help from ChatGPT I created a Python script that did some basic analysis. Let's run that over our contigs.

```
/home/jovyan/shared-team/seqanalysis_2024/qc_assembly.py spades_out/contigs.fasta 
```

Let's break down the parameters and results from this assembly QC report to understand what each value means:

- **Total Contigs: 103** This indicates that your genome assembly is made up of 103 contigs, which are contiguous sequences of DNA assembled from the data. Ideally, in a perfect assembly, you would have a single contig representing the entire genome. More contigs typically indicate fragmentation in the assembly.
- **Total Length: 1,916,571 bp (base pairs)** This is the total length of all the contigs combined, measured in base pairs (bp), which are the basic units of DNA (A, T, C, G). It gives an idea of how much of the genome has been assembled. Comparing this with the expected genome size can tell you how complete your assembly might be.
- **Largest Contig: 350,850 bp** The largest contig is 350,850 bp long. This value is useful because larger contigs indicate fewer gaps or unresolved regions in the assembly. It also suggests that a significant portion of the genome is captured in a single sequence, which is good for assembly quality.
- **N50: 212,467 bp** The N50 is a key metric in genome assembly that represents the length of the shortest contig in the set of contigs that together make up at least 50% of the total assembly length. A higher N50 value indicates better assembly quality, meaning that a smaller number of long contigs make up a large portion of the genome. In this case, it means that 50% of the total genome is contained in contigs of at least 212,467 bp.
- **GC Content: 30.78%** This refers to the percentage of guanine (G) and cytosine (C) bases in your genome assembly. Different organisms have different GC contents, and this value can be compared to the known GC content for the species you're working with. If your GC content is close to the expected range, it suggests that your assembly is likely accurate. A major deviation could indicate issues like contamination or assembly errors.


Now let's try running kraken over the assembly

```
kraken2 --threads 8 --db /home/jovyan/shared-public/db/kraken2/standard_8gb/latest --output kraken_assembly.hits.txt --report kraken_assembly.report.txt  --use-names  spades_out/contigs.fasta
```

What has happened to the results compared to what we see with reads??

---


## Long Reads

---


### QC on long reads 
---

Now we must turn our attention to the long reads. 

```
cd /home/jovyan/seqanalysis_example/long_reads
```

We will use `Nanoplot` and `PoreChop`

While somewhat old-fashioned, these remain useful tools for learning the basics of Nanopore data processing and quality control. They help researchers understand important steps like adapter trimming (Porechop) and quality metrics visualization (NanoPlot). However, both tools have largely been superseded by more modern developments integrated into Guppy and MinKNOW. 

 - **Guppy**, Oxford Nanopore's advanced basecaller, handles both basecalling and quality control more efficiently

 - **MinKNOW** now includes real-time adapter filtering, reducing the need for external tools like Porechop. 

Despite these advancements, learning these older tools provides foundational knowledge that is helpful for understanding how data processing has evolved and gives flexibility for handling legacy datasets or specialized workflows.

Let's make a directory for the NanoPlot results and then run it


```
mkdir nanoplot_out
NanoPlot --fastq DTP_1_1_Nano.fastq.gz  --plots  kde  dot --N50 -o nanoplot_out
```

note that the command `NanoPlot` is mixed-case and case-sensitive.

Take a look through the output files, starting with NanoStats.txt and NanoPlot-report.html

- **NanoStats.txt** gives you a quick numerical overview of the sequencing dataset's quality and composition.
- **NanoPlot-report.html** is a more detailed, interactive report that allows you to visualize the statistics and assess the overall quality of the sequencing run. Look for balanced read lengths, high-quality scores, and a consistent GC content as indicators of good data quality.
- What do you make of these results?

Let's now trim the reads using `porechop`

```
porechop -i DTP_1_1_Nano.fastq.gz -o DTP_1_1_Nano_chopped.fastq.gz
```

 - Watch the progress of the program on your terminal. This takes a long time. You should open a fresh terminal to continue below while waiting. But remember to activate the `seqanalysis` conda environment and `cd` to `/home/jovyan/seqanalysis_example/long_reads`

Once it has finished, let's look at what we have

```
ls -alh
```

How much difference did PoreChop make to file sizes?

 - *stretch target*: run NanoPlot again over the trimmed file, but keep directories tidy!

---

### Taxonomic profiling of long reads

---

Let's run kraken over these long reads.

```
kraken2 --threads 8 --db /home/jovyan/shared-public/db/kraken2/standard_8gb/latest --output nano.hits.txt --report nano_reads_report.txt  --use-names  DTP_1_1_Nano.fastq.gz 
```

And then `krona`


```
ktImportTaxonomy -t 5 -m 3 nano_reads_report.txt   -o nano_reads_KronaReport.html
```


What do we see now? Compare and contrast the view you get when looking at the nano_reads_report.txt file and the nano_reads_KronaReport.html file. 


---

### Assembly of long reads

---


The requirements of optimal assembly of long reads is slightly different from that of short reads. 
We will use the long read assembler `flye`.

**Why Flye is Preferred for Long Reads**

- *Error Handling*: Flye is better at handling the high error rates typical of raw long-read data.
- *Contiguity*: Flye produces more contiguous assemblies for long-read data, resolving large repeats and structural variations that are hard to assemble with short reads.
- *Long-Read Focus*: Flye is specifically designed for long-read assemblies, making it more efficient and accurate for this task than SPAdes, which was primarily designed for short reads.
- *No Need for Hybrid Assembly*: Flye works directly with long reads, while SPAdes often needs short reads to polish or scaffold long-read assemblies effectively


Let's make a directory to work in. Then copy the chopped nanopore reads across.


```
mkdir /home/jovyan/seqanalysis_example/long_reads/flye
cp /home/jovyan/seqanalysis_example/long_reads/DTP_1_1_Nano_chopped.fastq.gz /home/jovyan/seqanalysis_example/long_reads/flye
cd /home/jovyan/seqanalysis_example/long_reads/flye
```

Now let's run `flye`

```
flye --nano-raw DTP_1_1_Nano_chopped.fastq.gz --genome-size 1.5m --out-dir flye_output --threads 8
```

Take a look inside the `flye_output` directory. 

**Summary of Flye Output**

- *assembly.fasta*: The final genome assembly, the most critical file for downstream analysis.
- *assembly_info.txt*: Detailed stats for each contig (length, coverage).
- *scaffolds.fasta*: Larger scaffolds, if generated, with links between contigs.
- *flye.log*: Log of the assembly process, useful for troubleshooting.
- *graph.gfa*: The assembly graph, useful for visualization of complex regions.
- *stats.txt*: Quick summary of assembly metrics like N50 and total genome size.
 
 Let's just grab headers for contigs

```
grep ">" flye_out/assembly.fasta
```

 - What do you make of the output?
 
Let's run the Python script again.

```
/home/jovyan/shared-team/seqanalysis_2024/qc_assembly.py flye_out/assembly.fasta 
```

- what does that tell us?

Now let's run `kraken` and `krona` over the assembly. 

```
kraken2 --threads 8 --db /home/jovyan/shared-public/db/kraken2/standard_8gb/latest --output nano_assembly_kraken_hits.txt --report nano_assembly_kraken_report.txt  --use-names /home/jovyan/seqanalysis_example/long_reads/flye/flye_output/assembly.fasta
ktImportTaxonomy -t 5 -m 3 nano_assembly_kraken_report.txt   -o nano_assembly_KronaReport.html
```
And what do we see in the `krona` display now?

---

# And now it's over to you!

![](https://raw.githubusercontent.com/mmbdtp/mmbdtp.github.io/gh-pages/modules/sequencing/_posts/42b0af67-1cf9-4cf1-b705-919c772ff879.webp)

The sequences you yourselves created should be available in `/home/jovyan/shared-team/seqanalysis_2024`

Your mission is to analyse all of those sequences, performing QC, taxonomic profiling and assembly using the approaches outlined above. Take care to maintain a tidy file and directory structure as you go. 

You are required to write a report as a PDF document covering each sequence, detailing QC of reads, what's in the samples and how well the assembly worked. Include images from mages files or screen dumps if helpful, Include a short tabulated summary at the start of the report.

You shuld work in your pre-existing team of two and produce three reports, one for each team. Upload your reports into /home/jovyan/shared-team/seqanalysis_2024/reports

**Stretch target**: if you really race through all of this. Where short and long read sequences are available from the same sample, you could install `unicyler` and see if hybrid assembly gives better results than assemblies on short and long reads alone.

---

**Important**: Please fill in the [feedback form](https://forms.office.com/e/si0HtJNKD7) before you leave on friday!

---
