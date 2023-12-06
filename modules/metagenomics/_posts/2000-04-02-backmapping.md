---
title: Backmapping
---


## What is this about

Backmapping or "read recruitment" of raw reads against the assembly is the process of 
aligning the raw reads back onto the assembled genomes.

* **Accuracy Check**: By aligning the raw reads to the assembled genomes, scientists can verify how accurately the assembly represents the original genetic material. It's a quality control step. This step has different meaning if you backmap the reads of one sample against its assembly, or if you do a co-assembly. Can you think of the difference?
* **Quantifying Abundance**: It also aids in determining how abundant certain genomic sequences are in the sample by seeing how many raw reads align to specific regions of the assembly. **This is the foundation of binning!**

### How to Do It Using Minimap2?

Minimap2 is a fast program for aligning long DNA sequences like those found in genomics.

The command typically looks like 

```bash
minimap2 -a -x sr assembly.fasta reads_R1.fastq reads_R2.fastq | samtools sort -o aligned.bam -
samtools index aligned.bam
```

where:

* "*sr*" is a preset for short reads,
* *assembly.fasta* is your assembled genome, and 
* *reads.fastq* are your raw reads.

:bulb: The output can be used to evaluate the **coverage** of the assembly, for example using IGV.

[![IGV](https://camo.githubusercontent.com/340658f1bbad9a553dd9b3e8d943f6696cb4a825840ee22b65a5fe754cda2afa/68747470733a2f2f6d69726f2e6d656469756d2e636f6d2f6d61782f323733342f312a68616f374a4d4c516c6f71626d68412d6535793141512e706e67)](https://github.com/telatin/covtobed/wiki/Introduction)


## :memo: Your task

1. Backmap the reads of your sample against the assembly
2. Then backmap the reads of other 2 samples against the same assembly
3. Using samtools stats, evaluate the number of mapped reads:
    * How many reads of the selected sample mapped to the assembly?
    * How many reads from other samples mapped to the assembly?