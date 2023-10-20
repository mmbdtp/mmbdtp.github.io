---
layout: page
---

# Automated annotation of genome using Prokka 

`Prokka` can be run on galaxy, as we have done for `shovill` and `unicycler`, [https://galaxy.quadram.ac.uk/galaxy](https://galaxy.quadram.ac.uk/galaxy). You can run this on assembies generated by both `Shovill` and `Unicycler`. If you run `Prokka` in galaxy, you will need to download the output to your local computer to do the next part. 

You should use the assembled contigs we generated earlier, particularly the one generated with Unicycler.

If you had a problem with generating an assembly with Unicycler. Here are some results I prepared earlier. 

* [long_assembly.fasta](/seq-analysis/long_assembly.fasta)
* [long_assembly.gfa](/seq-analysis/long_assembly.gfa)
* [unicycler.log](/seq-analysis/unicycler.log)

### Tips for using command-line

Installing `prokka`
```
mamba create -y -n prokka prokka
conda activate prokka
```

Running `prokka`
```
prokka assembly.fasta
```

The important output files:

* PROKKA_11042022.gbk: Genbank format of the genome annotation
* PROKKA_11042022.gff: GFF format of the genome annotation

PROKKA keeps all the genbank records, one after the other, in a single file. *You will need to manually split the individual genbank records in the .gbk file.* You can find the different records by looking for the header. 

```
//
LOCUS       2                      10376 bp    DNA     linear       04-NOV-2022
DEFINITION  Genus species strain strain.
ACCESSION   
VERSION
```

### Comparing Prokka output with manual annotation

*Remember to save your manual annotations first!*. `File` > `Save entry as...` > `Genbank Format`. 

This can be done using `Artemis`. Load up the manual annotation you have already prepared. See the `Read an Entry...` option under `File`. Each set of annotations can be toggled in top left. 

![Artemis with two tracks]({{site.baseurl}}/seq-analysis/art_compare.png)

How did you annotation compare to the automated one?