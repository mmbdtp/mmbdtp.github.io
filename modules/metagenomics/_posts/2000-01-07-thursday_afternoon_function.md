---
title: Thursday afternoon - Function
---



# Thursday Afternoon 
## Function

---

## Objectives 

---

## Abricate 

**Abricate** is a bioinformatics tool developed by Torsten Seemann, designed for identifying antimicrobial resistance and virulence genes in genomic sequences by screening against curated databases. It supports a range of databases, making it a valuable tool for researchers working in microbial genomics and public health surveillance.

### Reference and Credits
- **Developer**: Torsten Seemann
- **Source and Documentation**: Detailed documentation and updates for **Abricate** can be found on [Torsten Seemann's GitHub](https://github.com/tseemann/abricate). 
- **Citations**: When using **Abricate** in your research, crediting Torsten Seemann and referencing the tool's GitHub page is encouraged to acknowledge the work and support its continued development.

Big thanks to Torsten Seemann for his contributions to bioinformatics and for creating user-friendly, impactful tools that empower the research community.

![](https://avatars.githubusercontent.com/u/453972?v=4)

### Running Abricate on FASTA Files

To screen all FASTA files in a directory, run:

```bash
abricate *.fasta
```

This command will process each `.fasta` file and display the results on the screen.

### Listing Available Databases

To view the databases available in **Abricate**:

```bash
abricate --list
```

This command will show installed databases and their descriptions, allowing you to select the appropriate one for your analysis.

### Saving Output to a TSV File

To direct **Abricate** output to a `.tsv` file for further analysis:

```bash
abricate *.fasta > abricate_out.tsv
```

This saves the output as `abricate_out.tsv`, keeping results accessible for later review.

### Selecting a Database

Specify a database for targeted analysis using the `--db` flag:

```bash
abricate --db <database_name> *.fasta > abricate_<database_name>_out.tsv
```

For example, to use **ResFinder**:

```bash
abricate --db resfinder *.fasta > abricate_resfinder_out.tsv
```

Run Abricate over your bins using various databases


### Interpreting Output

**Abricate** output columns include:

- **#FILE**: Input file name.
- **SEQUENCE**: Contig with the detected gene.
- **START/END**: Gene positions.
- **GENE**: Detected gene name.
- **COVERAGE/IDENTITY**: Gene coverage and match similarity.
- **DATABASE**: Source database.

### Database Descriptions

1. **ARD (Antibiotic Resistance Database)**: Identifies antibiotic resistance genes. [ARD GitHub](https://github.com/arpcard/ard-data)
2. **ResFinder**: Detects acquired antimicrobial resistance genes. [ResFinder](https://cge.food.dtu.dk/services/ResFinder/) (Zankari et al., 2012)
3. **MEGARes**: Profiles resistance genes across antibiotic classes. [MEGARes](https://megares.meglab.org/) (Doster et al., 2020)
4. **NCBI Pathogen Detection**: Detects resistance genes from public genome data. [NCBI Pathogen Detection](https://www.ncbi.nlm.nih.gov/pathogens/antimicrobial-resistance/)
5. **ARG-ANNOT**: Annotates resistance genes. [ARG-ANNOT](https://mediterranee-infection.com/article.php?laref=283&titre=arg-annot-a-database-for-the-annotation-of-antibiotic-resistance-genes-in-bacterial-genomes) (Gupta et al., 2014)
6. **PlasmidFinder**: Identifies plasmid types in bacterial genomes. [PlasmidFinder](https://cge.food.dtu.dk/services/PlasmidFinder/) (Carattoli et al., 2014)
7. **ECOH**: Identifies *E. coli* serotype genes. [ECOH at DTU](https://cge.food.dtu.dk/services/ECOH/)
8. **Ecoli_VF**: Detects *E. coli* virulence genes. [Ecoli_VF](https://www.mgc.ac.cn/cgi-bin/VFs/genus.cgi?Genus=Escherichia)
9. **VFDB**: Screens for virulence factors across bacteria. [VFDB](http://www.mgc.ac.cn/VFs/) (Chen et al., 2016)

