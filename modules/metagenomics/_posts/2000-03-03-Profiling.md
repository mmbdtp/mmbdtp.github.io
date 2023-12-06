---
title: Profiling
---

There are a variety of tools available for profiling metagenomes. In this context we use the term
**profiling** to specifically refer to the analysis of the metagenome content with a read-by-read
approach. Usually, we imply **taxonomic profiling** will be done: a report with the detected taxa
and their relative abundance in the sample will be generated/visualized. 
Alternatively, other types of profiling can be done, for example **functional profiling** (e.g. 
pathways).

## :gear: Common tools

- [Kraken2](https://ccb.jhu.edu/software/kraken2/): You already used Kraken2, which is fast and ships a good set of pre-built databases. Depending on the database, it can be very memory intensive
- [Metaphlan](https://github.com/biobakery/MetaPhlAn): Instead of a k-mer approach, will use mapping against a curated database. It's slower but more sensitive.
- [Kaiju](https://bioinformatics-centre.github.io/kaiju/): Less popular but accurate profiler
- [KMCP](https://bioinf.shenwei.me/kmcp/): A hybrid approach to maintain performance of k-mer based methods while increasing sensitivity

## :mag: Inspect the results

You already mastered how to run Kraken2 (and bracken!).
[Metagenomics databases](https://benlangmead.github.io/aws-indexes/k2) are often very large,
so we already performed the analysis on the full dataset (not the subsampled one), using different databases.

You will find the results in `/shared/team/week5/reports`:

* **kraken-std** (standard: Refeq archaea, bacteria, viral, plasmid, human1, UniVec_Core)
* **kraken-slim** (Standard with DB capped at 8 GB)
* **kraken-pluspfp** (Standard plus Refeq protozoa, fungi & plant)
* **kraken-nt** (Very large collection, inclusive of GenBank, RefSeq, TPA and PDB)

## :memo: Task

1. Inspect the output files with methods you already tried before (e.g. Krona, Pavian)
2. Try "MultiQC" with one of the databases

### MultiQC

[MultiQC](https://multiqc.info/) is a tool that aggregates results from bioinformatics analyses across many samples into a single report. It's very useful to quickly inspect the results of a pipeline, and it's very easy to use.

```bash
# Install MultiQC
mamba create -y -n multiqc python=3.9 multiqc
```

```bash
# Run multiqc on the kraken "nt" dataset
multiqc -o multiqc-nt /shared/team/week5/reports/kraken-nt/
```

Download the output report HTML and open it!
