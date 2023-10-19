---
title: Quality control for long reads
---

# Assess quality with Nanoplot - Long reads only
In case of long reads, we can check sequence quality with [Nanoplot](https://github.com/wdecoster/NanoPlot/). It provides basic statistics with nice plots for a fast quality control overview.


**If you are not using a CLIMB-BIG-DATA notebook, you will need to download the data.**
```
wget -O m64011_190830_220126.Q20.subsample.fastq.gz https://zenodo.org/records/5730295/files/m64011_190830_220126.Q20.subsample.fastq.gz?download=1

```

### Exercise 1: Run Nanoplot and FASTQC

```
NanoPlot --fastq  m64011_190830_220126.Q20.subsample.fastq.gz  --plots hex dot kde  --N50
```

**Inspect the generated HTML file**

**What is the mean Qscore?**

**What is the median, mean and N50?**

**Looking at “Read lengths vs Average read quality plot using dots plot”. Did you notice something unusual with the Qscore? Can you explain it?** 

**Repeat this with FASTQC**





### Citation

Some of this material was adapted from:

* Bérénice Batut, Maria Doyle, Alexandre Cormier, Anthony Bretaudeau, Laura Leroi, Erwan Corre, Stéphanie Robin, Erasmus+ Programme, Cameron Hyde, Quality Control (Galaxy Training Materials). https://training.galaxyproject.org/training-material/topics/sequence-analysis/tutorials/quality-control/tutorial.html Online; accessed Wed Oct 18 2023
* Hiltemann, Saskia, Rasche, Helena et al., 2023 Galaxy Training: A Powerful Framework for Teaching! PLOS Computational Biology 10.1371/journal.pcbi.1010752


[Back to Programme]({{site.baseurl}}/modules/sequencing/week-2-programme/).
