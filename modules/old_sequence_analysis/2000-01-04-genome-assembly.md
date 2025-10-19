---
title: Hybrid and short-read-only assembly of sequenced reads
---

# Hybrid and short-read-only assembly of sequenced reads

In this exercise, you will take the reads of our novel pathogen and assembly the genome using the short reads alone and a hybrid assembly using both long and short reads. This is to illustrate how differences in sequencing data (namely, read length) will affect the final outcome. We will use these assemblies in subsequent analyses.  

If you are unsure about files and file types, please review [Crash Course Computer Science episode 20](https://www.youtube.com/watch?v=KN8YgJnShPM&list=PL8dPuuaLjXtNlUrzyH5r6jN9ulIgZBpdo&index=21). The [Crash Course Computer Science episode 21](https://www.youtube.com/watch?v=OtDxDvCpPL4&list=PL8dPuuaLjXtNlUrzyH5r6jN9ulIgZBpdo&index=22) is also helpful in understanding compression (.gz, .zip). The [episode on operating systems](https://www.youtube.com/watch?v=26QPDBe-NB8&list=PL8dPuuaLjXtNlUrzyH5r6jN9ulIgZBpdo&index=19) will also help you understand `Unix` a little better. 

Here is the contrived scenario about our novel pathogen:

> An outbreak of diarrhoea has been reported, affecting over a hundred people attending a political conference. Most have fever, severe abdominal cramps and bloody diarrhoea. Five have been admitted to an ICU and are in a critical condition. You have been presented with the sequencing output from what appears to be an *E. coli* strain that has been isolated from the faeces of one of the ICU patients. Over the coming days you must QC, assemble and annotate the genome sequence from this strain, looking for unusual mobile genetic elements, which should be analysed in depth.

## What is a genome assembler doing?

Genome assemblers assume the following about sequenced reads:

* Reads are resolved into nucleotide bases (ATGC & ambiguous base calls)
* Reads are randomly distributed across the target DNA, and
* Reads are represent an oversampling of the target DNA, such that individual reads repeatedly overlap
* Genome assemblers calculate overlaps between reads and (usually) represent as a graph/network. Then “walk” the graph to determine the original sequence.

Genome assemblers calculate overlaps between reads and (usually) represent as a graph/network. Then “walk” the graph to determine the original sequence.

See [Torsten Seemann’s slides on de novo genome assembly](https://tinyurl.com/torstaseembler)

![Assembly overview]({{site.baseurl}}/seq-analysis/assembler.jpg)

> Whole genome shotgun sequencing: Genome is sheared into small approximately equal sized fragments which are subsequently small enough to be sequenced in both directions followed by cloning. The cloned sequences (reads) are then fed to an assembler (illustrated in Figure 2). b To overcome some of the complexity of normal shotgun sequencing of large sequences such as genomes a hierarchical approach can be taken. The genome is broken into a series of large equal segments of known order which are then subject to shotgun sequencing. The assembly process here is simpler and less computationally expensive. From [Commins, Toft and Fares 2009](http://dx.doi.org/10.1007/s12575-009-9004-1)
 
## What is R1 and R2?
Just as a reminder, many sequencing library preparation kits include an option to generate so-called “paired-end reads“.  Intact genomic DNA is sheared into several million short DNA fragment.  Individual reads can be paired together to create paired-end reads, which offers some benefits for downstream bioinformatics data analysis algorithms.  The structure of a paired-end read is shown here.

![read overview]({{site.baseurl}}/seq-analysis/r1r2.jpg)

“Read 1”, often called the “forward read”, extends from the “Read 1 Adapter” in the 5′ – 3′ direction towards “Read 2” along the forward DNA strand.

“Read 2”, often called the “reverse read”, extends from the “Read 2 Adapter” in the 5′ – 3′ direction towards “Read 1” along the reverse DNA strand.


## How genome assemblers fail perfection
In theory, Genome assembly software with perfect reads of good length will  reconstruct the genome verbatim 

However, Sequencing platform have errors (and cause errors downstream): 

* Struggle with GC rich and/or AT rich DNA.
* Have lower read quality towards the end of reads (5', 3' or both ends)
* Have difficulty reading homopolymers (e.g. AAAAA or TTTTTTT) accurately
* **Have read lengths that does not span repeated sequences in the genome**

 ![Assembly graph]({{site.baseurl}}/seq-analysis/assembly_graph.png)


* Repeats: A segment of DNA that occurs more than once in the genome
* Read length must span the repeat

##### Outcomes of your final contigs

 ![Repeats]({{site.baseurl}}/seq-analysis/repeats.jpg)

> Mate-pair signatures for collapse style mis-assemblies. (a) Two copy tandem repeat R shown with properly sized and oriented mate-pairs. (b) Collapsed tandem repeat shown with compressed and mis-oriented mate-pairs. (c) Two copy repeat R, bounding unique sequence B, shown with properly sized and oriented mate-pairs. (d) Collapsed repeat shown with compressed and mis-linked mate-pairs. From [https://doi.org/10.1186%2Fgb-2008-9-3-r55](https://doi.org/10.1186%2Fgb-2008-9-3-r55)

##### How to span repeats:
* Long reads (ONT, Pacbio)
* Long reads (Sanger)
* Optical mapping
* Hi-C
* Or just don’t! 


## Short read assembly with Shovill
About Shovill: The [SPAdes genome assembler](https://github.com/ablab/spades) has become the de facto standard de novo genome assembler for Illumina whole genome sequencing data of bacteria and other small microbes. 

SPAdes was a major improvement over previous assemblers like Velvet, but some of its components can be slow and it traditionally did not handle overlapping paired-end reads well. 

Shovill is a pipeline which uses SPAdes at its core, but alters the steps before and after the primary assembly step to get similar results in less time. Shovill also supports other assemblers like SKESA, Velvet and Megahit, so you can take advantage of the pre- and post-processing the Shovill provides with those too. 

Warning: Shovill is for isolate data only, primarily small haploid organisms. It will NOT work on metagenomes or larger genomes. Please use Megahit directly instead. See more details at [https://github.com/tseemann/shovill](https://github.com/tseemann/shovill)

### Doing short read assembly with Shovill
Is Shovill installed? Please see the [Installing software]({{site.baseurl}}/modules/sequence-analysis/download-data/) section.

We can run Shovill with:
```
shovill  --R1 novel-pathogen_R1.fastq.gz  \
         --R2 novel-pathogen_R2.fastq.gz   \
         --outdir short_read_only --force
```

The final output: 

```
[shovill] Repaired 0 contigs from spades.fasta at 0 positions.
[shovill] Assembly is 4938311, estimated genome size was 4712883 (+4.78%)
[shovill] Using genome graph file 'spades/assembly_graph_with_scaffolds.gfa' => 'contigs.gfa'
[shovill] Walltime used: 3 min 6 sec
[shovill] Results in: /home/ubuntu/code/mmbdtp.github.io/temp/short_read_only
[shovill] Final assembly graph: /home/ubuntu/code/mmbdtp.github.io/temp/short_read_only/contigs.gfa
[shovill] Final assembly contigs: /home/ubuntu/code/mmbdtp.github.io/temp/short_read_only/contigs.fa
[shovill] It contains 297 (min=75) contigs totalling 4938311 bp.
[shovill] More correct contigs is better than fewer wrong contigs.
[shovill] Done.
```

The contents of the `short_read_only` folder: 

```
(shovill) ubuntu@chomp:~/code/mmbdtp.github.io/temp$ ls short_read_only/ -hl
total 15M
-rw-rw-r-- 1 ubuntu ubuntu 4.9M Nov  2 17:01 contigs.fa
-rw-rw-r-- 1 ubuntu ubuntu 4.8M Nov  2 17:01 contigs.gfa
-rw-rw-r-- 1 ubuntu ubuntu    0 Nov  2 17:01 shovill.corrections
-rw-rw-r-- 1 ubuntu ubuntu 365K Nov  2 17:01 shovill.log
-rw-rw-r-- 1 ubuntu ubuntu 4.8M Nov  2 17:01 spades.fasta
```

The important output files, please review each of these:

* contigs.fa: The final assembly you should use
* contigs.gfa: Assembly graph (spades)
* shovill.log: Full log file for bug reporting


## Hybrid assembly with Unicyler 

There are different programs for hybrid assembly, with slightly different results. So, why [Unicycler](https://github.com/rrwick/Unicycler)?

* All in one package - Good hybrid assembly
* Can use short read only and bridge gaps with some guesswork 
* A lot of options on how aggressive you want it to join contigs
* Easy install
* Checks for circularisation 


### Doing hybrid assembly with Unicyler 

Is Unicyler installed? Please see the [Installing software]({{site.baseurl}}/modules/sequence-analysis/download-data/) section. 

We can run Unicyler with:
```
unicycler -1 novel-pathogen_R1.fastq.gz \
  -2 novel-pathogen_R2.fastq.gz \
  -l novel-pathogen-long-reads.fastq.gz  \
  -o hybrid_assembly
```

The contents of the `hybrid_assembly` folder: 

```
001_spades_graph_k027.gfa
001_spades_graph_k053.gfa
001_spades_graph_k071.gfa
001_spades_graph_k087.gfa
001_spades_graph_k099.gfa
001_spades_graph_k111.gfa
001_spades_graph_k119.gfa
001_spades_graph_k127.gfa
002_depth_filter.gfa
003_overlaps_removed.gfa
004_long_read_assembly.gfa
005_bridges_applied.gfa
006_final_clean.gfa
assembly.fasta
assembly.gfa
unicycler.log
```

At this stage, you may want to ask about what's different about a hybrid assembly?

The important output files:

* assembly.fasta: The final assembly you should use
* assembly.gfa: Assembly graph 
* unicycler.log: Full log file for bug reporting

# Exercise 1: Short read and hybrid assembly

## Theory questions

**Define "Read 1" and "Read 2" in the context of paired-end sequencing. What is the direction of extension for each?**

**Explain the term "homopolymers" and why they are difficult to read accurately in sequencing.**

**Why is it important for read lengths to span repeated sequences in the genome during assembly?**

**What issues do some sequencing platforms encounter when dealing with GC-rich or AT-rich DNA sequences?**

## Practical questions

**Assemble the genome of our novel pathogen using short reads only** 

**Assemble the genome of our novel pathogen using both long and short reads (as a hybrid assembly)**

_We will compare the results of these assemblies in the next exercise_


[Answers to the exercises](/seq-analysis/genome-assembly-answers)

 
 [Back to Programme]({{site.baseurl}}/modules/sequence-analysis/programme/).