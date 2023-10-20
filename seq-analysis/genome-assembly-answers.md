---
layout: page
---


**Assemble the genome of our novel pathogen using short reads only** 

If you have a problem with this process. Here are some results I prepared earlier. 

* [contigs.fa](/seq-analysis/contigs.fa)
* [contigs.gfa](/seq-analysis/contigs.gfa)
* [shovill.corrections](/seq-analysis/shovill.corrections)
* [shovill.log](/seq-analysis/shovill.log)


**Assemble the genome of  our novel pathogen using both long and short reads (as a hybrid assembly)**

If you had a problem with generating an assembly with Unicycler. Here are some results I prepared earlier. 

* [long_assembly.fasta](/seq-analysis/long_assembly.fasta)
* [long_assembly.gfa](/seq-analysis/long_assembly.gfa)
* [unicycler.log](/seq-analysis/unicycler.log)



_We will compare the results of these assemblies in the next exercise_

**Define "Read 1" and "Read 2" in the context of paired-end sequencing. What is the direction of extension for each?**

Read 1 (Forward Read):

* Read 1 is often referred to as the "forward read."
* It extends from the "Read 1 Adapter" in the 5' to 3' direction.
* In paired-end sequencing, it represents the sequence information obtained from one end of the DNA fragment.
* The extension direction of Read 1 is along the forward DNA strand, meaning it reads the DNA sequence starting from the 5' end and extending towards the 3' end of the DNA fragment.

Read 2 (Reverse Read):

* Read 2 is often referred to as the "reverse read."
* It extends from the "Read 2 Adapter" in the 5' to 3' direction.
* In paired-end sequencing, it represents the sequence information obtained from the other end of the same DNA fragment as Read 1.
* The extension direction of Read 2 is along the reverse DNA strand, meaning it reads the DNA sequence starting from the 5' end and extending towards the 3' end of the DNA fragment, which is opposite to the direction of Read 1.

**Explain the term "homopolymers" and why they are difficult to read accurately in sequencing.**

**Why is it important for read lengths to span repeated sequences in the genome during assembly?**

**What issues do some sequencing platforms encounter when dealing with GC-rich or AT-rich DNA sequences?**