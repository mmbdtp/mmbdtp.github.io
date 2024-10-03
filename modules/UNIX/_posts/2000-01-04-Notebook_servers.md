---
title: Tuesday Morning — Notebook servers and Conda Environments
---

# Tuesday Morning — Notebook servers and Conda Environments 

## Handling files and commands within a Jupyter Notebook 

### How to launch and access a notebook server

* First, follow the instructions on how to get CLIMB-BIG-DATA's web interface 
* Using the navigation menu on the left hand side, select 'Notebook servers' under the 'Compute' subheading
* Click the 'Launch notebook server' green action button on the right hand side
* Select a 'Standard server'
* Click 'Launch Server' and monitor the progress bar
* Once ready, click the url beneath the 'User notebook server'
* On first login, you may be asked to authorize access to your Bryn account. Click 'Authorize'
* The JupyterLab interactive computing interface should open in a new tab.

---

### Finding your way around the JupyterLab interface

Spend a few moments familiarizing yourself with the basic JupyterLab interface. Head on over to the [JupyterLab interface docs](https://jupyterlab.readthedocs.io/en/stable/user/interface.html) to get started.

Fundamentally, your screen is divided into a few areas. You'll see context menus at the top (File, Edit, View, Run etc.), a file browser pane on the left, and an activity area that initially displays a launcher interface with tiles. Clicking on one of these tiles will open a new tab in the activity area.

---


### The Terminal
The terminal allows us to access a system shell (just like on ypur MacBooks), so let's start there.

Select File -> New -> Terminal, or click the Terminal icon on the launcher pane. You'll get a new terminal tab in the activity bar, and find yourself in a bash shell.

---


### Who is jovyan?
Looking at your bash prompt, you'll notice that your username is jovyan (there's a backstory, but it means 'related to Jupyter'). Why is everyone's username the same? Your notebook server is running as a container. The container instance is private and linked to your Bryn user's storage, but the image it runs is the same for everyone. As a result, it is not necessary or desirable to have unique system users.

TLDR: don't worry about it. Inside your notebook server, your username is jovyan

---

### Where am I? Who am I?#

By default, you're in a bash shell running against the base operating system of the climb-jupyterhub container image (which is based on a flavour of Linux called Ubuntu). You'll see in your bash prompt that you're in your home directory (represented by the tilde character ~).


```
jovyan:~$ pwd
```

```
/home/jovyan
```

---


## Handling Conda Environments

In this tutorial, we will cover the importance of using **Conda** for managing environments, especially when working in Jupyter Notebooks or complex data science and bioinformatics projects.

---

### What is Conda?

**Conda** is an open-source package management and environment management system. It allows you to easily install, update, and manage software packages, as well as create isolated environments for different projects. Conda supports multiple programming languages, such as Python, R, Ruby, and others, making it a versatile tool for various domains.

---

### Why is Conda Useful?

Conda is especially useful in the following scenarios:

- **Environment Isolation**: Conda helps you create isolated environments where you can install different versions of the same software without conflicts. This is crucial in projects that require different libraries or dependencies that may not work well together.
  
- **Package Management**: Conda simplifies the installation of software packages, whether they are Python libraries, system libraries, or software written in other programming languages.

- **Cross-Platform Compatibility**: Conda works across various operating systems, including Linux and Windows. This ensures consistency when working on collaborative projects with team members on different systems or when deploying solutions on servers.

- **Jupyter Notebooks**: When working in a Jupyter Notebook environment, Conda allows you to create and switch between environments effortlessly. This is helpful when running notebooks that require different software packages or specific versions of libraries without causing issues across the system.

---

### Conda vs Other Package Managers

Compared to other package managers, Conda has a few distinct advantages:

- **Language-Agnostic**: Unlike `pip`, which primarily manages Python packages, Conda can handle packages written in multiple languages.
  
- **Environment Management**: While tools like `virtualenv` only isolate Python environments, Conda manages the entire environment, including system libraries.

- **Package Compatibility**: Conda ensures that all dependencies are compatible with one another, reducing the risk of installation issues or conflicting versions. This is especially crucial for scientific computing or bioinformatics projects, which often require various tools to work together seamlessly.

---

### Setting Up Conda Environments

Conda simplifies installing packages, but a common issue arises with **conflicting versions**:

- You may want to use a new version of a program like`samtools`, but another tool or pipeline might require an older version, say `samtoolsv1.1`.
- A typical bioinformatics workflow involves dozens of different tools, each requiring specific libraries and dependencies.
- Installing everything in one environment can result in conflicts between versions of the same tool.

To solve this, Conda allows you to create **isolated environments**, where each environment has its own set of packages, libraries, and configurations, independent of other environments. This ensures that tools in one environment don’t conflict with those in another.

---

### Creating and Managing Conda Environments

Conda makes it easy to manage software and environments, helping you keep your projects organized and free from conflicts.


### Default .condarc#

When you first launch a notebook server, we generate a default .condarc in your home directory. This sets the path for your new environments to 
```
/shared/team/conda/$JUPYTER_USERNAME. 
```

Why? You are working in a team and your home directory is relatively small vs your team share, so it makes sense to use the larger mount. In additon, it becomes easy to share conda environments with other team members.

```
cat ~/.condarc
envs_dirs:
  - /shared/team/conda/demouser.andy-bryn-dev-t
[...]
```
---

### Creating new conda environments

Understanding the above, you can create new conda environments in the usual way. The only caveat is that if you wish to use this environment with ipython notebooks, you must install `ipykernel`.

**Warning**: *If you don't install `ipykernel` in a new conda environment, it won't show up on the launcher or be available to select within the python notebooks interface. However, you can use the environment just fine within a terminal.*

Let's start with an environment called `bioinfo_fun`. 

```
conda create -n bioinfo_fun
conda activate bioinfo_fun
```

If you try listing channels, you'll see you already have conda-forge and bioconda set.

```
conda config --show channels
```

Let's remember to install ipykernel in this environment

```
conda install ipykernel -y
```
---

### A quick analysis of an *E. coli* genome

Let's do a quick analysis of an *E. coli* genome


```
conda install -y -c bioconda seqkit
```

And let's download and analyse a real genome

```
wget "https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000005845.2/download?include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT&filename=GCF_000005845.2.zip" -O coligenome.zip
unzip coligenome.zip

seqkit stats -T -a -G -i ncbi_dataset/data/GCF_000005845.2/GCF_000005845.2_ASM584v2_genomic.fna > coli_genome_stats.tsv

```

Using the menu for the JupyterLab interface, select the `New Launcher` option. 
Now have a play with the interface's graphical user interface. Find and take a look at the files you have downloaded and unzipped and the .tsv file showing the results of the analysis.


---


Time for a break!

---

