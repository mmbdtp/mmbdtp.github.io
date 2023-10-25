---
title: Detecting virulence factors
---

# Pathogen genomics

### Day 4

- Exercise: [Detecting virulence factors](/seq-analysis/detect_vir) in our genome, using web tools (including resfinder etc).
- Additional Exercise: [Detecting virulence factors](/seq-analysis/detect_vir_cli) in our genome using command line tools.



# Detecting virulence factors and AMR

If you had a problem with generating an assembly. Here are some results I prepared earlier. 

* [long_assembly.fasta](/seq-analysis/long_assembly.fasta)

##### In this exercise, use the Galaxy or other web based tools to locate virulence factors and anitmicrobial resistance in a genome. 

With the decreasing costs, whole genome sequencing has become a viable antimicrobial resistance surveillance tool. Several methods and tools have been published in recent years for detecting genetic determinants of antimicrobial resistance from whole-genome sequencing (WGS) and whole-metagenome sequencing (WMS) data. 

Each of these tools are performing some kind of alignment or search in your sequence of interest. The program are looking for known virulence/AMR genes in published databases. The tools may accept sequence reads or assembled contigs are input. The databases are build with slightly different so the results may not be the same when using different tools. See review for more details [Boolchandani, D'Souza & Dantas, (2019)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6525649/).

The tool and the database are usually detacted. So you can run `ARIBA` and `abricate` (tools) with the CARD database. 

It is easy to believe that running your sequence of interest against as many databases are possible will give you the best information. It may not. You may wind up with a number of conflicting reports and no way to decide which one is correct. You must understand your chosen databases and determine whether it is suitable for your work. 

There are numerous databases for different gene panels - AMR, virulence, serotype, etc.

* [NCBI AMRFinderPlus](https://journals.asm.org/doi/10.1128/AAC.00483-19)
* [CARD](https://academic.oup.com/nar/article/45/D1/D566/2333912)
* [Resfinder](https://academic.oup.com/jac/article/67/11/2640/707208)
* [ARG-ANNOT](https://journals.asm.org/doi/10.1128/AAC.01310-13)
* [VFDB](https://academic.oup.com/nar/article/44/D1/D694/2503049)
* [PlasmidFinder](https://journals.asm.org/doi/10.1128/AAC.02412-14)
* [EcOH](https://www.microbiologyresearch.org/content/journal/mgen/10.1099/mgen.0.000064)
* [MEGARES 2.00](https://academic.oup.com/nar/article/48/D1/D561/5624973)

If you have a particular interest in AMR, please review the content from this 
[Workshop on Antimicrobial Resistance](https://www.climb.ac.uk/amr-workshop/)


#### Software to try

In Galaxy try:

* [abricate](https://github.com/tseemann/abricate) - with different databases

What does the output actually mean? See the documentation, [abricate](https://github.com/tseemann/abricate)

```
#FILE	SEQUENCE	START	END	GENE	COVERAGE	COVERAGE_MAP	GAPS	%COVERAGE	%IDENTITY	DATABASE	ACCESSION	PRODUCT
test_sample	1	610199	614052	entF	1-3854/3882	===============	0/0	99.28	95.67	vfdb	NP_752604	(entF) enterobactin synthase multienzyme complex component ATP-dependent [Enterobactin (VF0228)] [Escherichia coli CFT073]
test_sample	1	615427	616242	fepC	1-816/816	===============	0/0	100.00	97.30	vfdb	NP_752606	(fepC) ferrienterobactin ABC transporter ATPase [Enterobactin (VF0228)] [Escherichia coli CFT073]
test_sample	1	616239	617231	fepG	1-993/993	===============	0/0	100.00	94.06	vfdb	NP_752607	(fepG) iron-enterobactin ABC transporter permease [Enterobactin (VF0228)] [Escherichia coli CFT073]
```
N.B. The co-ordinates in your genome may not match the co-ordinates above

Can you locate these genes in `Artemis`? Use the automated annotation - Do these annotations agree?

Other online tools you can try, include: 

* FimTyper: [https://cge.food.dtu.dk/services/FimTyper/](https://cge.food.dtu.dk/services/FimTyper/)
* ResFinder: [https://cge.food.dtu.dk/services/ResFinder/](https://cge.food.dtu.dk/services/ResFinder/)

Using these tools what can you say about the genome?

[Back to Programme]({{site.baseurl}}/modules/sequence-analysis/programme/).



# Detecting virulence factors - CLI
Is `abricate` installed? Please see the [Installing software](seq-analysis/installing) section. 

If you had a problem with generating an assembly. Here are some results I prepared earlier. 

* [long_assembly.fasta](/seq-analysis/long_assembly.fasta)

Try using `abricate` on the command line with the different databases

```
abricate oh/assembly.fasta 
```


You can use `--list` to see what databases are available. 

```
abricate --list 
DATABASE	SEQUENCES	DATE
argannot	1749	2018-Jul-16
card	2220	2018-Jul-16
ecoh	597	2018-Jul-16
ncbi	4324	2018-Jul-16
plasmidfinder	263	2018-Jul-16
resfinder	2280	2018-Jul-16
vfdb	2597	2018-Jul-16

```

The output will be the same as with the web-based example. What does it mean? Try using `grep` or `cut` to filter the results.

```
#FILE	SEQUENCE	START	END	GENE	COVERAGE	COVERAGE_MAP	GAPS	%COVERAGE	%IDENTITY	DATABASE	ACCESSION	PRODUCT
test_sample	1	610199	614052	entF	1-3854/3882	===============	0/0	99.28	95.67	vfdb	NP_752604	(entF) enterobactin synthase multienzyme complex component ATP-dependent [Enterobactin (VF0228)] [Escherichia coli CFT073]
test_sample	1	615427	616242	fepC	1-816/816	===============	0/0	100.00	97.30	vfdb	NP_752606	(fepC) ferrienterobactin ABC transporter ATPase [Enterobactin (VF0228)] [Escherichia coli CFT073]
test_sample	1	616239	617231	fepG	1-993/993	===============	0/0	100.00	94.06	vfdb	NP_752607	(fepG) iron-enterobactin ABC transporter permease [Enterobactin (VF0228)] [Escherichia coli CFT073]
```

[Back to Programme]({{site.baseurl}}/modules/sequence-analysis/programme/).



# Detecting plasmids and prophages
There are a number of tools for detecting plasmids and prophage. Some of these are on galaxy. You can apply these tools to your assembled genome.

If you had a problem with generating an assembly. Here are some results I prepared earlier. 

* [long_assembly.fasta](/seq-analysis/long_assembly.fasta)

I have provided another plasmid genome, which is totally unrelated to our data. You can run this too to see how results can differ with the various tools:

* [some_plasmid.fasta](/seq-analysis/some_plasmid.fasta)

### Plasmid detection 

* [MOB-Suite](https://github.com/phac-nml/mob-suite). Try this one.
* [Plasmidfinder](https://cge.food.dtu.dk/services/PlasmidFinder/) - Web tool. Try this one.
* [Abricate](https://github.com/tseemann/abricate) (plasmidfinder). Try this one.
* [ARIBA](https://github.com/sanger-pathogens/ariba) (plasmidfinder)
* [PLACNET](https://castillo.dicom.unican.es/upload/) - Web tool. Try this one.
* [COPLA](https://castillo.dicom.unican.es/copla_guide/) - Web tool. Try this one.
* [PlasFlow](https://github.com/smaegol/PlasFlow)

### Phages

* [PHAST/PHASTER](https://phaster.ca/) - Web tool. Try this one.
* [VirSorter 2](https://github.com/jiarong/VirSorter2)
* [DeepVirFinder](https://github.com/jessieren/DeepVirFinder)

#### About PHASTER

PHASTER is a web based tool and can be quite slow. We are showing it as how you can approach characterising a genomic region of interest. 

Have a look at an example [https://phaster.ca/submissions/NC_000913](https://phaster.ca/submissions/NC_000913)

How PHASTER works - Annotation
* Glimmer, tRNAscan-SE, ARAGORN
* DBSCAN for finding clustered genes

How PHASTER works - Finding prophages
* At least four viral genes
* Cumulative length of the gaps between component clusters
* Number and proportion of genes matching known prophage genes
* Presence of blast hits that match a high proportion of a particular phage strain’s known genes
* Presence of IS and viral genes with key functions including integrases transposases, and structural genes.

Arndt D, Marcu A, Liang Y, Wishart DS. PHAST, PHASTER and PHASTEST: Tools for finding prophage in bacterial genomes. Brief Bioinform. 2019;20(4):1560-1567. doi:[10.1093/bib/bbx121](https://academic.oup.com/bib/article/20/4/1560/4222653)

[Back to Programme]({{site.baseurl}}/modules/sequence-analysis/programme/).



# Detecting plasmids - CLI

Is `abricate` installed? Please see the [Installing software](seq-analysis/installing) section. 

If you had a problem with generating an assembly. Here are some results I prepared earlier. 

* [long_assembly.fasta](/seq-analysis/long_assembly.fasta)

I have provided another plasmid genome, which is totally unrelated to our data. You can run this too to see how results can differ with the various tools:

* [some_plasmid.fasta](/seq-analysis/some_plasmid.fasta)

Try using `abricate` on the command line with the different databases

```
abricate oh/assembly.fasta --db plasmidfinder
```


You can use `--list` to see what databases are available. 

```
abricate --list 
DATABASE	SEQUENCES	DATE
argannot	1749	2018-Jul-16
card	2220	2018-Jul-16
ecoh	597	2018-Jul-16
ncbi	4324	2018-Jul-16
plasmidfinder	263	2018-Jul-16
resfinder	2280	2018-Jul-16
vfdb	2597	2018-Jul-16

```

The output will be the same as with the web-based example. What does it mean? Try using `grep` or `cut` to filter the results.

```
Using database plasmidfinder:  263 sequences -  2018-Jul-16
#FILE	SEQUENCE	START	END	GENE	COVERAGE	COVERAGE_MAP	GAPS	%COVERAGE	%IDENTITY	DATABASE	ACCESSION	PRODUCT
Processing: oh/assembly.fasta
Found 1 genes in oh/assembly.fasta
oh/assembly.fasta	2	8821	8951	ColRNAI_1	1-130/130	========/======	1/1	100.00	87.02	plasmidfinder	DQ298019	ColRNAI_1__DQ298019
```

You can also try other tools such as: 

* [MOB-Suite](https://github.com/phac-nml/mob-suite).





[Back to Programme]({{site.baseurl}}/modules/sequence-analysis/programme/).