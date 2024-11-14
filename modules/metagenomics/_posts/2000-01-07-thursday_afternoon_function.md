---

title: Thursday Afternoon - Function

---

## Thursday Afternoon - Function

### Annotating and Analysing MAGs for Function

---

## Objectives 

By the end of this session, you will be able to:

1. **Understand the Role of Annotation Tools**:
   - Grasp the significance of annotation in microbial genomics.
   - Differentiate between functional annotation and general genome annotation.

2. **Run and Interpret Results from Bioinformatics Tools**:
   - Use **Abricate** for identifying antimicrobial resistance and virulence genes in metagenome-assembled genomes (MAGs).
   - Execute **Prokka** for annotating prokaryotic genomes, identifying coding sequences, tRNAs, and rRNAs.
  
3. **Configure and Run Tools on Provided Datasets**:
   - Set up and activate appropriate Conda environments for running **Abricate** and **Prokka**
   - Practice using command-line options to customize analysis (e.g., selecting specific databases, setting output prefixes).
   - Annotate a genome bin (e.g., `maxbin_bins/001.fasta`) using all three tools and interpret the output files.

4. **Evaluate and Discuss Results**:
   - Interpret annotation results to identify genes and pathways, noting differences between hypothetical and annotated proteins.
   - Explore metabolic summaries and discuss how they relate to the ecological roles of the studied microbial genomes.
   - Reflect on findings, including the detection of resistance genes and metabolic capabilities.

5. **Develop Skills in Bioinformatics Workflows**:
   - See how **Abricate** and **Prokka** fit into a comprehensive functional analysis.
   - Use additional flags and parameters to tailor outputs for specific research needs.
   - Navigate output directories and interpret key files, such as `.tsv`, `.gbk`, and summary reports.

6. **Enhance Critical Analysis**:
   - Compare findings from different tools to understand how they complement each other.
   - Ask and discuss questions related to detected functions, pathways, and potential ecological implications.


---

## Abricate 

**Abricate** is a bioinformatics tool developed by Torsten Seemann for identifying antimicrobial resistance and virulence genes in genomic sequences by screening against curated databases. It is widely used in microbial genomics and public health surveillance.

---

### Reference and Credits

- **Developer**: Torsten Seemann
- **Source and Documentation**: [Torsten Seemann's GitHub](https://github.com/tseemann/abricate)
- **Citations**: Acknowledge Torsten Seemann in research using **Abricate** to support its continued development.

Big thanks to Torsten Seemann for creating practical and useful tools that contribute significantly to the research community.

![](https://avatars.githubusercontent.com/u/453972?v=4)


---

### Running Abricate on FASTA Files

To screen all FASTA files in a directory:

```bash
abricate *.fasta
```

This command processes each `.fasta` file and displays results on the screen.

---

### Listing Available Databases

To list databases installed in **Abricate**:

```bash
abricate --list
```

This will show the available databases and their descriptions, helping you choose the appropriate one for your analysis.

---

### Saving Output to a TSV File

To redirect **Abricate** output to a `.tsv` file for further review:

```bash
abricate *.fasta > abricate_out.tsv
```

This saves results to `abricate_out.tsv` without printing to the screen.

---

### Selecting a Database

Use the `--db` flag to specify a database for targeted analysis:

```bash
abricate --db <database_name> *.fasta > abricate_<database_name>_out.tsv
```

For instance, to use the **ResFinder** database:

```bash
abricate --db resfinder *.fasta > abricate_resfinder_out.tsv
```

---

### Interpreting Output

**Abricate** output in TSV format includes:

- **#FILE**: Name of the input file.
- **SEQUENCE**: Contig containing the detected gene.
- **START/END**: Positions of the gene.
- **GENE**: Name of the detected gene.
- **COVERAGE/IDENTITY**: Percentage of gene covered and match similarity.
- **DATABASE**: Database where the gene was identified.

---

### Database Descriptions

1. **ARD (Antibiotic Resistance Database)**: Detects antibiotic resistance genes. [ARD GitHub](https://github.com/arpcard/ard-data)
2. **ResFinder**: Identifies acquired antimicrobial resistance genes. [ResFinder](https://cge.food.dtu.dk/services/ResFinder/) - Zankari et al., 2012.
3. **MEGARes**: Comprehensive resistance gene profiling. [MEGARes](https://megares.meglab.org/) - Doster et al., 2020.
4. **NCBI Pathogen Detection**: Resistance genes from public genome data. [NCBI Pathogen Detection](https://www.ncbi.nlm.nih.gov/pathogens/antimicrobial-resistance/)
5. **ARG-ANNOT**: Curated resistance gene annotation. [ARG-ANNOT](https://mediterranee-infection.com/article.php?laref=283&titre=arg-annot-a-database-for-the-annotation-of-antibiotic-resistance-genes-in-bacterial-genomes) - Gupta et al., 2014.
6. **PlasmidFinder**: Identifies plasmid replicon types. [PlasmidFinder](https://cge.food.dtu.dk/services/PlasmidFinder/) - Carattoli et al., 2014.
7. **ECOH**: Detects *Escherichia coli* serotype genes. [ECOH at DTU](https://cge.food.dtu.dk/services/ECOH/)
8. **Ecoli_VF**: Screens for *E. coli* virulence genes. [Ecoli_VF](https://www.mgc.ac.cn/cgi-bin/VFs/genus.cgi?Genus=Escherichia)
10. **VFDB**: Comprehensive virulence factor detection. [VFDB](http://www.mgc.ac.cn/VFs/) - Chen et al., 2016.



---

# Task
Spend the next 20-30 minutes using abricate to explore your MAGs. Do you find many resistance genes or virulence factors??

---
### Prokka: Genome Annotation Step-by-Step

---

## Overview
**Prokka** is a bioinformatics tool designed for the rapid annotation of prokaryotic genomes, producing outputs that adhere to standard file formats. It is commonly used to identify coding sequences (CDS), tRNAs, rRNAs, and other genomic features, annotating them based on known databases.

Prokka is another product created by the mighy Torsten Seemann

If you use Prokka results in your work, cite Seemann T (2014) Prokka: rapid prokaryotic genome annotation. Bioinformatics. 30(14):2068-9.

---

## Setting Up the Environment

**Prokka** can be finickety when installed in different Conda environments. I have found that for best results, it's good to use the `seqanalysis` environment.

### Step 1: Activate the Environment
```bash
conda deactivate
conda activate seqanalysis
conda install prokka
```


---


### Step 2: Test Prokka Installation
Run these commands to confirm the installation:

- **Check Prokka's help screen**:
  ```bash
  prokka
  ```

- **Check the version**:
  ```bash
  prokka --version
  ```

- **List installed databases**:
  ```bash
  prokka --listdb
  ```

---

## Running Prokka on a Genome Bin

To annotate a genome bin, use the following command:

```bash
prokka maxbin_bins.001.fasta
```

This command will generate an output directory named based on the current date (e.g., `PROKKA_11132024`).

### Example Log Output
The output log provides detailed information about the annotation process. You will see information about:

- **Prokka version and environment details**.
- **Number of contigs** and their total base pair count.
- **tRNA and rRNA predictions**.
- **Coding sequence (CDS) prediction** and annotation.
- **Annotation progress**, including searching against known protein databases.

### Key Sections Explained:
- **Annotation Summary**: Shows the total number of predicted features, such as CDS, tRNAs, and rRNAs.
- **Search Methods**: Prokka utilizes external tools like **Prodigal**, **Barrnap**, and **BLAST** for gene prediction and database searches.
- **Output Files**:
  - **.gff**: A standard feature file.
  - **.gbk**: GenBank format.
  - **.faa**: Protein sequences.
  - **.ffn**: Nucleotide sequences of genes.
  - **.fna**: Nucleotide sequences of contigs.
  - **.txt**: Summary statistics.

---

### Additional Options

**Prokka** provides several options to customize the annotation process. Here are some useful flags:

- **Specify a Prefix for Output Files**:
  By default, Prokka uses a prefix based on the date. You can change this with the `--prefix` option to make outputs easier to track:

  ```bash
  prokka --prefix my_annotation maxbin_bins.001.fasta
  ```

  This will name the output files with `my_annotation` instead of `PROKKA_<date>`.

- **Use a Reference Genome for Enhanced Annotation**:
  If you have a related reference genome that can guide the annotation, you can use the `--proteins` flag:

  ```bash
  prokka --proteins reference_proteins.faa maxbin_bins.001.fasta
  ```

  This helps Prokka prioritize annotation based on known proteins, which can improve accuracy.

- **Set the Locus Tag**:
  Use the `--locustag` flag to define a custom locus tag prefix for your gene IDs:

  ```bash
  prokka --locustag ABC123 maxbin_bins.001.fasta
  ```

- **Genus-Specific Annotation**:
  The `--genus` flag allows Prokka to apply more targeted rules for annotation if the genus is known:

  ```bash
  prokka --genus Escherichia maxbin_bins.001.fasta
  ```

- **Number of CPUs**:
  To speed up the annotation, increase the number of CPU cores used:

  ```bash
  prokka --cpus 8 maxbin_bins.001.fasta
  ```

These options allow you to tailor the annotation process to your specific project needs, improving both the customization and accuracy of your results.


---

### Reviewing the output files:
After running Prokka, explore the generated directory (e.g., `PROKKA_11132024`) and examine the output files, particularly files of the form `*.gbk`.
Explore the files, understand the annotations, and consider how different databases and thresholds affect the final output.

---

## Discussion Point
- Why are only some proteins annotated with functions while others are merely labeled as hypothetical?
  
