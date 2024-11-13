---

title: Thursday Afternoon - Function

---

## Function

---

## Objectives 

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


---

### Task
Spend the next 20-30 minutes using abricate to explore your MAGs. Do you find many resistance genes or virulence factors??

---



  
10. **VFDB**: Comprehensive virulence factor detection. [VFDB](http://www.mgc.ac.cn/VFs/) - Chen et al., 2016.

---
