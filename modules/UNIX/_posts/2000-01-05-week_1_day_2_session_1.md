---
title: Notebook servers
---

## Handling files and commands within a Jupyter Hub 

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

### Finding your way around the JupyterLab interface#

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

### What about sudo?
jovyan doesn't have sudo privileges. This may seem restrictive, but we've pre-configured the climb-jupyter base image with everything you'd likely need sudo for pre-installed. Everything else should be installable via package managers, such as conda. 

You'll also be able to run ``Nextflow``, a workflow management system, specifically designed to handle and orchestrate computational pipelines popular in the bioinformatics community for its ability to streamline the analysis of large datasets.'out-of-the-box'.

---

### How do I install software?
Its critical to understand how conda works with notebook servers as things are not quite the same as on a traditional Unix box or a MacBook.

Firstly, understand that you cannot install new software to the base conda environment. Lets have a look at why.

```
conda info --envs
base                     /opt/conda
```

The base conda env is installed at /opt/conda. Since we are running instide a container, any changes made to this part of the filesystem will not be retained once the container is stopped and restarted (unlike your home dir and shares, which are persisted).

We've made the base environment read only to prevent any confustion.

**Tip**: You must create a new conda environment before installing software!

---

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

###Creating new conda environments#

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

###A quick analysis of an *E. coli* genome

Let's do what we did yesterday on your Macs, but here in this new sophisticated setting!


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

[Back to the Programme for this week](week_1__programme.md)
