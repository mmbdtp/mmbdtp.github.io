---
title: Thursday - Session 3
---

# Introduction to JupyterHub

>**Things covered here:**
>
> - Overview of Jupyter Notebooks
> - Running Python in a Notebook
> - Running R in a Notebook

## Notes and Code

Jupyter Notebooks are powerful because you are able to write notes as you run your commands. This is perfect for data exploration as you can write about what you are interested in exploring as you find new cool things within your data.

## Markdown

Jupyter notebook allows basic formatting following the Markdown syntax. You need to change the code block to interpret as MD not as Python.

### Markdown Syntax

- **Headers**
`#`, `##`, `##`

- **Bold**
`**text**`

- **Italics**
`*text*`

- **List**
`1. 1. 1.`
`- - -`
`1. 2. 3.`

- **Table**
`| Name | Type |`
`|--|--|`
`|Sam|Bioinformatician|`

- **Links**
`[my fav website](intranet.nbi.ac.uk)`

## Example Python analysis

Lets say you are a huge fan of Candidatus Carsonella. It just so happens to have one of the smallest known genomes and you want to while away the time staring at its annotated genome.

Let's display it using a python notebook. We'll need to download the genome information from genebank and then use a python module to display it.

### Creating the conda environment

```bash
mamba create -n gene_viewer biopython dna_features_viewer ipykernel
```

Now you should be able to use this conda environment as a basis of your Python Kernel.

### Pulling from GeneBank

```python
# Load python modules 
from Bio import Entrez # Python package to interact with NCBI Search engine API
from Bio import SeqIO # Library to manipulate genomic sequences
```

```python
# Ask Entrez what NCBI databases it knows
Entrez.email = "sam.haynes@quadram.ac.uk" # Entrez requires you to identify yourself
stream = Entrez.einfo()
result = stream.read()
stream.close()
print(result)
```

```python
# Download genebank file for species of interest
stream = Entrez.efetch(db="nucleotide", id="CP003543", rettype="gb", retmode="text")
record = SeqIO.read(stream, "genbank")
stream.close()
record.seq 
```

### Plotting a genome

```python
# load packages
from dna_features_viewer import BiopythonTranslator # module to read genebanl files and create graphics
```

```python
graphic_record = BiopythonTranslator().translate_record(record)
ax, _ = graphic_record.plot(figure_width=10, strand_in_label_threshold=7)
```

## Example R analysis

Jupyter can also run R as a kernal! Which is great as R is so good for creating plots.

### Dummy R project

```r
# Load all the tidyverse R packages
library(tidyverse)

ggplot(mpg, aes(displ, hwy, colour = class)) +
  geom_point()
```

```r
data = mpg |> group_by(manufacturer) |> summarise(n = n())

ggplot(data, aes(manufacturer, n)) +
  geom_col()
```
