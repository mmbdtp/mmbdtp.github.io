---
title: Thursday morning - Taxonomy
---


# Thursday morning 
## Taxonomy

---


## Objectives 


---


## GTDB toolkit

[GTDB-Tk](https://ecogenomics.github.io/GTDBTk/installing/index.html)




[Installation](https://ecogenomics.github.io/GTDBTk/installing/bioconda.html)

**Create the GTDB-Tk environment**

```
mamba create -n gtdbtk-2.1.1 -c conda-forge -c bioconda gtdbtk=2.1.1
```

**Activate the GTDB-Tk conda environment**

```
conda activate gtdbtk-2.1.1
```

**Access the GTDB-Tk reference data**

GTDB-Tk requires ~110G of external data that needs to be downloaded and unarchived. I have done this for you already and out it here: `/home/jovyan/shared-team/gtdbtk_data/release220`
You need to add this to your path.

```
export GTDBTK_DATA_PATH=/home/jovyan/shared-team/gtdbtk_data/release220
```

**Fixing numpy**

But now if you try to run GTDB-Tk, you will get an error message

```
AttributeError: module 'numpy' has no attribute 'bool'.
`np.bool` was a deprecated alias for the builtin `bool`. To avoid this error in existing code, use `bool` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.bool_` here.
The aliases was originally deprecated in NumPy 1.20; for more details and guidance see the original release note at:
    https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
```

This means that you need to downgrade numpy for gtdb-tk to work.

```
pip install numpy==1.19.5
```

You should get this reponse.

```
Collecting numpy==1.19.5
  Downloading numpy-1.19.5-cp38-cp38-manylinux2010_x86_64.whl.metadata (2.0 kB)
Downloading numpy-1.19.5-cp38-cp38-manylinux2010_x86_64.whl (14.9 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 14.9/14.9 MB 68.8 MB/s eta 0:00:00
Installing collected packages: numpy
  Attempting uninstall: numpy
    Found existing installation: numpy 1.24.4
    Uninstalling numpy-1.24.4:
      Successfully uninstalled numpy-1.24.4
Successfully installed numpy-1.19.5
```

**Running GTDB-Tk**

We will build on [this example](https://ecogenomics.github.io/GTDBTk/examples/classify_wf.html) to run GTDB-Tk over our MAGs.

***Step 1: Gene calling (identify)***

```
gtdbtk identify --genome_dir ./. --out_dir /tmp/gtdbtk/identify --extension fasta --cpus 16
```

You should get something like this

```
[2024-11-13 08:49:44] INFO: GTDB-Tk v2.1.1
[2024-11-13 08:49:44] INFO: gtdbtk identify --genome_dir ./. --out_dir /tmp/gtdbtk/identify --extension fasta --cpus 16
[2024-11-13 08:49:44] INFO: Using GTDB-Tk reference data version r220: /home/jovyan/shared-team/gtdbtk_data/release220
[2024-11-13 08:49:45] INFO: Identifying markers in 3 genomes with 16 threads.
[2024-11-13 08:49:45] TASK: Running Prodigal V2.6.3 to identify genes.
[2024-11-13 08:49:58] INFO: Completed 3 genomes in 12.92 seconds (4.31 seconds/genome).
[2024-11-13 08:49:58] TASK: Identifying TIGRFAM protein families.            
[2024-11-13 08:50:02] INFO: Completed 3 genomes in 4.12 seconds (1.37 seconds/genome).
[2024-11-13 08:50:02] TASK: Identifying Pfam protein families.               
[2024-11-13 08:50:02] INFO: Completed 3 genomes in 0.37 seconds (8.01 genomes/second).
[2024-11-13 08:50:02] INFO: Annotations done using HMMER 3.1b2 (February 2015).
[2024-11-13 08:50:02] TASK: Summarising identified marker genes.
[2024-11-13 08:50:02] INFO: Completed 3 genomes in 0.08 seconds (38.54 genomes/second).
[2024-11-13 08:50:02] INFO: Done.
```

Results

The called genes and marker information can be found under each genomes respeective intermediate files directory, as shown below.

```
ls /tmp/gtdbtk/identify/identify/intermediate_results/marker_genes/maxbin_bins.001/
```

```
maxbin_bins.001_pfam_tophit.tsv         maxbin_bins.001_protein.faa.sha256  maxbin_bins.001_tigrfam.out                maxbin_bins.001_tigrfam.tsv.sha256
maxbin_bins.001_pfam_tophit.tsv.sha256  maxbin_bins.001_protein.fna         maxbin_bins.001_tigrfam.out.sha256         prodigal_translation_table.tsv
maxbin_bins.001_pfam.tsv                maxbin_bins.001_protein.fna.sha256  maxbin_bins.001_tigrfam_tophit.tsv         prodigal_translation_table.tsv.sha256
maxbin_bins.001_pfam.tsv.sha256         maxbin_bins.001_protein.gff         maxbin_bins.001_tigrfam_tophit.tsv.sha256
maxbin_bins.001_protein.faa             maxbin_bins.001_protein.gff.sha256  maxbin_bins.001_tigrfam.tsv
```

Let's just read the summary files which detail markers identified from the bacterial 120 marker set.

```
cat /tmp/gtdbtk/identify/identify/gtdbtk.bac120.markers_summary.tsv
```

```
name    number_unique_genes     number_multiple_genes   number_multiple_unique_genes    number_missing_genes    list_unique_genes       list_multiple_genes     list_multiple_unique_genes list_missing_genes
maxbin_bins.001 40      6       0       74      PF00410.20,PF00466.21,TIGR00019,TIGR00061,TIGR00083,TIGR00086,TIGR00092,TIGR00115,TIGR00116,TIGR00158,TIGR00166,TIGR00168,TIGR00250,TIGR00344,TIGR00362,TIGR00382,TIGR00398,TIGR00414,TIGR00435,TIGR00456,TIGR00580,TIGR00631,TIGR00663,TIGR00810,TIGR01011,TIGR01017,TIGR01059,TIGR01079,TIGR01082,TIGR01169,TIGR01171,TIGR01394,TIGR01632,TIGR01951,TIGR02027,TIGR02191,TIGR02273,TIGR02432,TIGR03625,TIGR03632     TIGR00006,TIGR00059,TIGR00065,TIGR00138,TIGR00967,TIGR02397             PF00380.20,PF01025.20,PF02576.18,PF03726.15,TIGR00020,TIGR00029,TIGR00043,TIGR00054,TIGR00064,TIGR00082,TIGR00084,TIGR00088,TIGR00090,TIGR00095,TIGR00186,TIGR00194,TIGR00337,TIGR00392,TIGR00396,TIGR00416,TIGR00420,TIGR00431,TIGR00436,TIGR00442,TIGR00445,TIGR00459,TIGR00460,TIGR00468,TIGR00472,TIGR00487,TIGR00496,TIGR00539,TIGR00593,TIGR00615,TIGR00634,TIGR00635,TIGR00643,TIGR00717,TIGR00755,TIGR00922,TIGR00928,TIGR00959,TIGR00963,TIGR00964,TIGR01009,TIGR01021,TIGR01029,TIGR01032,TIGR01039,TIGR01044,TIGR01063,TIGR01066,TIGR01071,TIGR01087,TIGR01128,TIGR01146,TIGR01164,TIGR01302,TIGR01391,TIGR01393,TIGR01510,TIGR01953,TIGR02012,TIGR02013,TIGR02075,TIGR02350,TIGR02386,TIGR02729,TIGR03263,TIGR03594,TIGR03654,TIGR03723,TIGR03725,TIGR03953
maxbin_bins.002 57      23      0       40      PF00380.20,PF00466.21,PF01025.20,TIGR00006,TIGR00019,TIGR00029,TIGR00043,TIGR00054,TIGR00061,TIGR00064,TIGR00086,TIGR00092,TIGR00095,TIGR00158,TIGR00166,TIGR00194,TIGR00344,TIGR00362,TIGR00392,TIGR00398,TIGR00420,TIGR00435,TIGR00442,TIGR00456,TIGR00459,TIGR00468,TIGR00496,TIGR00593,TIGR00615,TIGR00631,TIGR00635,TIGR00643,TIGR00663,TIGR00717,TIGR00755,TIGR00922,TIGR00964,TIGR00967,TIGR01029,TIGR01039,TIGR01066,TIGR01071,TIGR01164,TIGR01169,TIGR01510,TIGR01632,TIGR01953,TIGR02013,TIGR02027,TIGR02075,TIGR02191,TIGR02397,TIGR02432,TIGR03263,TIGR03594,TIGR03625,TIGR03725     PF00410.20,TIGR00082,TIGR00250,TIGR00382,TIGR00414,TIGR00431,TIGR00472,TIGR00580,TIGR00810,TIGR01009,TIGR01017,TIGR01021,TIGR01044,TIGR01079,TIGR01146,TIGR01171,TIGR01393,TIGR01394,TIGR02012,TIGR02729,TIGR03632,TIGR03654,TIGR03953             PF02576.18,PF03726.15,TIGR00020,TIGR00059,TIGR00065,TIGR00083,TIGR00084,TIGR00088,TIGR00090,TIGR00115,TIGR00116,TIGR00138,TIGR00168,TIGR00186,TIGR00337,TIGR00396,TIGR00416,TIGR00436,TIGR00445,TIGR00460,TIGR00487,TIGR00539,TIGR00634,TIGR00928,TIGR00959,TIGR00963,TIGR01011,TIGR01032,TIGR01059,TIGR01063,TIGR01082,TIGR01087,TIGR01128,TIGR01302,TIGR01391,TIGR01951,TIGR02273,TIGR02350,TIGR02386,TIGR03723
maxbin_bins.003 47      14      0       59      PF00380.20,TIGR00019,TIGR00020,TIGR00029,TIGR00064,TIGR00082,TIGR00083,TIGR00088,TIGR00116,TIGR00158,TIGR00166,TIGR00186,TIGR00344,TIGR00362,TIGR00398,TIGR00416,TIGR00431,TIGR00435,TIGR00436,TIGR00445,TIGR00459,TIGR00460,TIGR00487,TIGR00496,TIGR00539,TIGR00631,TIGR00634,TIGR00643,TIGR00663,TIGR00922,TIGR00959,TIGR00964,TIGR01011,TIGR01039,TIGR01066,TIGR01082,TIGR01302,TIGR01510,TIGR01951,TIGR01953,TIGR02075,TIGR02273,TIGR02350,TIGR02397,TIGR03594,TIGR03723,TIGR03953        TIGR00006,TIGR00043,TIGR00054,TIGR00095,TIGR00115,TIGR00138,TIGR00337,TIGR00382,TIGR00420,TIGR00755,TIGR01128,TIGR01391,TIGR01394,TIGR03725                PF00410.20,PF00466.21,PF01025.20,PF02576.18,PF03726.15,TIGR00059,TIGR00061,TIGR00065,TIGR00084,TIGR00086,TIGR00090,TIGR00092,TIGR00168,TIGR00194,TIGR00250,TIGR00392,TIGR00396,TIGR00414,TIGR00442,TIGR00456,TIGR00468,TIGR00472,TIGR00580,TIGR00593,TIGR00615,TIGR00635,TIGR00717,TIGR00810,TIGR00928,TIGR00963,TIGR00967,TIGR01009,TIGR01017,TIGR01021,TIGR01029,TIGR01032,TIGR01044,TIGR01059,TIGR01063,TIGR01071,TIGR01079,TIGR01087,TIGR01146,TIGR01164,TIGR01169,TIGR01171,TIGR01393,TIGR01632,TIGR02012,TIGR02013,TIGR02027,TIGR02191,TIGR02386,TIGR02432,TIGR02729,TIGR03263,TIGR03625,TIGR03632,TIGR03654
```

