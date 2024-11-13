


# Thursday Morning - Taxonomy

---

## Objectives

By the end of this tutorial, students will:
1. Understand the importance and use of the Genome Taxonomy Database (GTDB) and GTDB-Tk.
2. Set up and run GTDB-Tk to classify metagenome-assembled genomes (MAGs).
3. Interpret the results from GTDB-Tk output files.
4. Explore and utilize the GTDB website for taxonomy research and data exploration.

---

## Introduction to GTDB and GTDB-Tk

### The Genome Taxonomy Database (GTDB)

**GTDB** is a comprehensive resource that provides a standardized, phylogenetically consistent taxonomy for bacterial and archaeal genomes. It is the result of significant efforts to unify and update the classification of microbial genomes, addressing the inconsistencies present in traditional taxonomies. The GTDB has been instrumental in helping researchers better understand the evolutionary relationships among prokaryotes and refine classifications based on genome-based data.

- **Developer and Credits**: Phil Hugenholtz and colleagues at the Australian Centre for Ecogenomics deserve immense credit for creating and maintaining the GTDB. Their contributions have been pivotal in reshaping microbial taxonomy.
- **Explore GTDB**: Students are encouraged to visit the [GTDB website](https://gtdb.ecogenomic.org/) to browse genome data, search classifications, and explore phylogenetic trees. This hands-on exploration will provide insights into how GTDB organizes genomic data.

### What is GTDB-Tk?

**GTDB-Tk (Genome Taxonomy Database Toolkit)** is a software tool designed to classify genomes against the GTDB. It aligns genome data, identifies marker genes, and places genomes within a reference tree, offering reliable taxonomic classifications.

- **How it Works**: GTDB-Tk identifies key marker genes in input genomes, aligns these markers, and places the genome into a reference tree to deduce the most probable taxonomic classification.
- **Key Concepts**: 
  - **Marker Genes**: GTDB-Tk uses a curated set of marker genes (e.g., `bac120` for bacteria, `ar53` for archaea) essential for taxonomic placement.
  - **Phylogenetic Placement**: The tool integrates input data with the GTDB reference to place genomes on the phylogenetic tree accurately.


**Credit**: Phil Hugenholtz and the GTDB team have significantly impacted microbial taxonomy, providing researchers worldwide with valuable resources and tools.

 ![](https://scmb.uq.edu.au/sites/scmb.uq.edu.au/files/styles/uq_core_small_portrait/public/ckfinder/images/staff_profile/14.jpeg?itok=Emv-T1Z1)

---

## Setting Up GTDB-Tk

### Step 1: Install GTDB-Tk

**Installation Command**:
```bash
mamba create -n gtdbtk-2.1.1 -c conda-forge -c bioconda gtdbtk=2.1.1
```

**Activate the Environment**:
```bash
conda activate gtdbtk-2.1.1
```

---

### Step 2: Access GTDB-Tk Reference Data

GTDB-Tk requires external reference data (~110 GB). For this course, the data has already been downloaded and stored here:

```bash
export GTDBTK_DATA_PATH=/home/jovyan/shared-team/gtdbtk_data/release220
```

Ensure that the `GTDBTK_DATA_PATH` is correctly set to avoid data path errors.

---

### Step 3: Fixing Numpy Compatibility Issues

You might encounter an error related to `numpy` when running GTDB-Tk. To fix this, downgrade `numpy`:

**Command**:
```bash
pip install numpy==1.19.5
```

Verify that `numpy` has been successfully downgraded.

---

## Running GTDB-Tk

### Step 1: Gene Calling (Identify)

**Run the `identify` Command**:
```bash
gtdbtk identify --genome_dir ./. --out_dir /tmp/gtdbtk/identify --extension fasta --cpus 16
```

**Expected Output**:
You should see progress logs indicating the identification of marker genes, which will look like:

```
[2024-11-13 08:49:44] INFO: GTDB-Tk v2.1.1
[2024-11-13 08:49:44] INFO: Identifying markers in 3 genomes with 16 threads.
[2024-11-13 08:49:45] TASK: Running Prodigal V2.6.3 to identify genes.
```

**Results**:
The identified marker genes and intermediate files will be stored in the `identify` directory:

```bash
ls /tmp/gtdbtk/identify/identify/intermediate_results/marker_genes/maxbin_bins.001/
```

---

### Step 2: Aligning Genomes (Align)

**Run the `align` Command**:
```bash
gtdbtk align --identify_dir /tmp/gtdbtk/identify --out_dir /tmp/gtdbtk/align --cpus 16
```

**Expected Output**:
You will see logs related to the alignment of identified markers and concatenation of sequences:

```
[2024-11-13 09:27:37] INFO: Aligning markers in 3 genomes with 16 CPUs.
[2024-11-13 09:31:20] INFO: Masked bacterial alignment from 41,084 to 5,035 AAs.
```

**Results**:
Alignment results, including `gtdbtk.bac120.msa.fasta.gz`, will be saved in the `align` directory.

---

### Step 3: Classifying Genomes (Classify)

**Run the `classify` Command**:
```bash
gtdbtk classify --genome_dir ./. --align_dir /tmp/gtdbtk/align --out_dir /tmp/gtdbtk/classify -x fasta --cpus 16
```

**Expected Output**:
Logs showing the placement of genomes into the reference tree and classification output:

```
[2024-11-13 09:40:00] INFO: Classifying genomes based on the aligned data.
```

**Results**:
The main output files are `gtdbtk.bac120.summary.tsv` and `gtdbtk.ar53.summary.tsv`, detailing genome classifications.

---

## Exploring GTDB and GTDB-Tk Results

### Hands-On Task:
1. **Run GTDB-Tk over a sample MAG** provided in the course data.
2. **Explore the GTDB website** to learn more about how microbial taxonomy is structured and how your classified genome fits within it.

**Discussion Points**:
- Why is GTDB's approach to taxonomy more consistent than traditional methods?
- What do the markers and classifications reveal about your MAG?
- How does the GTDB tree structure provide insight into evolutionary relationships?

