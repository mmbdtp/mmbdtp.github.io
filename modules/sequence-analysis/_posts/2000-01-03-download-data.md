---
title: Preparing our data and environment
---


# Downloading simulated data 

*Reads are available at [https://zenodo.org/records/10018484](https://zenodo.org/records/10018484)*

You can directly download them with `wget` or similar. 

```
wget -O novel-pathogen-long-reads.v3.fastq.gz https://zenodo.org/records/10018484/files/novel-pathogen-long-reads.v3.fastq.gz?download=1
wget -O novel-pathogen_R1.fastq.gz https://zenodo.org/records/10018484/files/novel-pathogen_R1.fastq.gz?download=1
wget -O novel-pathogen_R2.fastq.gz https://zenodo.org/records/10018484/files/novel-pathogen_R2.fastq.gz?download=1
```

The files are compressed (.gz) FASTQ files. There are three files: 

* Short read R1 - [novel-pathogen_R1.fastq.gz](https://zenodo.org/records/10018484/files/novel-pathogen_R1.fastq.gz?download=1)
* Short read R2 - [novel-pathogen_R2.fastq.gz](https://zenodo.org/records/10018484/files/novel-pathogen_R2.fastq.gz?download=1) 
* Long reads - [novel-pathogen-long-reads.fastq.gz](https://zenodo.org/records/10018484/files/novel-pathogen-long-reads.v3.fastq.gz?download=1)


### Reminder: What is FASTQ

FASTQ format is a text-based format for storing both a biological sequence (usually nucleotide sequence) and its corresponding quality scores. Both the sequence letter and quality score are each encoded with a single ASCII character for brevity.

It was originally developed at the Wellcome Trust Sanger Institute to bundle a FASTA formatted sequence and its quality data, but has become the de facto standard for storing the output of high-throughput sequencing instruments.

### Reminder: What is Gzip 

A GZ file is an archive file compressed by the standard GNU zip (gzip) compression algorithm. It typically contains a single compressed file but may also store multiple compressed files. gzip is primarily used on Unix operating systems for file compression.

[Back to Programme]({{site.baseurl}}/modules/sequence-analysis/programme/).


# Check environment, software, and tools ready for later exercises

We recommend installing command-line bioinformatics software through the package manager `conda`.

> If you are using a CLIMB-BIG-DATA notebook, this is already installed for you. You can skip this step.

`Conda` is an open source package management system and environment management system that runs on Windows, macOS, Linux and z/OS. `Conda` quickly installs, runs and updates packages and their dependencies. `Conda` easily creates, saves, loads and switches between environments on your local computer. It was created for `Python` programs, but it can package and distribute software for any language.

* The installation instructions for [macOSX can be found here](https://docs.conda.io/projects/conda/en/latest/user-guide/install/macos.html)
* The installation instructions for [Linux can be found here](https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html)

Andrea has an extended guide here: [https://telatin.github.io/microbiome-bioinformatics/Install-Miniconda/](https://telatin.github.io/microbiome-bioinformatics/Install-Miniconda/). 

### Switching to `mamba` 

Conda has recently integrated a faster "solver" called `mamba`. You can enjoy the faster install times by setting `mamba` as the default solver. As shown below.

```
conda update -n base conda
conda install -n base conda-libmamba-solver
conda config --set solver libmamba
```

You need `conda` version 22.11 and above for this to work. 

###### In this course, you should use conda or containers (from week one) to manage your software


### Install the software for week 3 
Week four will also need certain software, please install these packages for week for via conda/mamba. 

```
mamba create -n week3 -c bioconda -c conda-forge prokka unicycler shovill
```

### Installing Artemis, Bandage and Mauve 

`Artemis`, `bandage` and `mauve` are software with a graphical interface you **should run on your local computer**. Follow the instructions at:

* [https://rrwick.github.io/Bandage/](https://rrwick.github.io/Bandage/)
* [https://darlinglab.org/mauve/user-guide/viewer.html](https://darlinglab.org/mauve/user-guide/viewer.html)
* [https://www.sanger.ac.uk/tool/artemis/](https://www.sanger.ac.uk/tool/artemis/)

###### The following are tips and examples to help you complete the tasks above 

### Installing software via conda (shovill)
This an example of how to install `shovill` in it's own environment. This is an example if you are not clear on the syntax. If the tools above are working, you should be fine for further exercises. 

```
conda create -n shovill shovill -y -c bioconda
conda activate shovill
```

You can install more programs in a single environment, as below:
```
conda create -n myproject shovill samtools -y -c bioconda
conda activate myproject
```

You can add more software to an existing environment, as below:
```
conda activate myproject 
conda install blast -y 
```

### Tips on organising your projects long term

A quick aside, "how should I organise my projects?" in the long term. 

* Use `conda` for package management (https://docs.conda.io/en/latest/)
* `Jupyter` notebooks (offered via CLIMB=BIG-DATA but free available for other setups) for exploring data and plotting figures (https://jupyter.org/)

For Processing large datasets

* Use workflow languages (`nextflow`)
* [Bactopia](https://bactopia.github.io/) is a good all-included workflow to start


### Some guidance on getting Jupyter notebooks up and running

> This is for future reference, and is not covered in this course.

You can Install and control R and jupyter notebooks through `conda`.

You can naturally change the name of the `conda env` (I used mapdemo here) to anything you like. I use mamba here to speed up the install process. I highly recommend mamba! I explain mamba in more detail here.

The first mamba install line is to install `jupyter` notebook, the second is for `R`, the `R` kernel for `jupyter` and common `R` packages `dplyr` and `ggplot`. The third `mamba` install line is for more specific `R` packages I want to use.

```
conda create -n mapdemo mamba
conda activate mapdemo
mamba install -y -c conda-forge pip notebook  nb_conda_kernels  jupyter_contrib_nbextensions
mamba install -y -c conda-forge r r-irkernel r-ggplot2 r-dplyr
mamba install -y -c conda-forge r-sf  r-ggrepel  r-cowplot r-maps
```

#### Starting the notebook
Once these are all installed you can start the `jupyter` notebook from a diretory of your choosing. Here I just make a demo directory

```
mkdir demo
jupyter notebook
```

You will then see the `jupyter` service start up and it will tell you where you can access it i.e. `http://localhost:8888/`

```
(mapdemo) ubuntu@chomp:~/code/demo$ jupyter notebook
[I 10:36:03.954 NotebookApp] [nb_conda_kernels] enabled, 8 kernels found
[I 10:36:04.186 NotebookApp] [jupyter_nbextensions_configurator] enabled 0.4.1
[I 10:36:04.188 NotebookApp] Serving notebooks from local directory: /home/ubuntu/code/demo
[I 10:36:04.188 NotebookApp] Jupyter Notebook 6.4.11 is running at:
[I 10:36:04.188 NotebookApp] http://localhost:8888/
[I 10:36:04.188 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
```

[Back to Programme]({{site.baseurl}}/modules/sequence-analysis/programme/).