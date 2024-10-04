---
title: Tuesday Morning — Notebook servers and Conda Environments
---

# Tuesday Morning — Notebook servers and Conda Environments 

## Handling files and commands within a Jupyter Notebook Server

Although your MacBooks run a version of Unix, most bioinformaticians prefer to use Linux and so most programs and pipelines work best on Linux. In bioinformatics, most researchers also prefer to use remote virtual servers over their own personal (bare metal) machines for several important reasons:
 - Bioinformatics analyses, such as genome assembly, sequence alignment, or large-scale data mining, can require substantial computational resources (e.g., CPU power, memory, and storage). 
- Personal computers, particularly laptops, often do not have the processing power or memory to handle these large datasets effectively. 
- By contrast, remote servers provide access to high-performance computing resources, which are specifically optimized for demanding tasks, ensuring faster and more efficient data processing. Here we are using a cloud platform, CLIMB-BIG-DATA, offering dynamic scalable resources.
- Virtualization allows multiple users to share the same physical hardware while operating in isolated environments, making it a more cost-effective solution. 
- Virtual servers can be easily scaled up or down based on demand, meaning researchers only pay for the computing power they use, which is especially beneficial when dealing with fluctuating project needs. Virtual servers also offer enhanced redundancy and fault tolerance, meaning that if hardware fails, another server can quickly take over without data loss or significant downtime. This reliability is crucial for long-running bioinformatics computations, which could otherwise be disrupted by hardware failures.

We will be using a Jupyter notebook server, which is an easy-to-access web-based platform that allows users to create and interact with Jupyter Notebooks, which in turn are documents that can contain live code, equations, visualizations, and text. The server runs the computational backend that provides command line access to a Linux shell, but can also processes code cells written in languages like Python, R, or Julia, and returns the output to the browser interface where users can view and manipulate results.

Before we get stuck in to using a Notebook server, let's set the scene with a short Powerpoint presentation.

### How to launch and access a notebook server

* !Need to add instructions on how to get to CLIMB-BIG-DATA's web interface! 
* Using the navigation menu on the left hand side, select 'Notebook servers' under the 'Compute' subheading
* Click the 'Launch notebook server' green action button on the right hand side
* Select a 'Standard server'
* Click 'Launch Server' and monitor the progress bar
* Once ready, click the url beneath the 'User notebook server'
* On first login, you may be asked to authorize access to your Bryn account. Click 'Authorize'
* The JupyterLab interactive computing interface should open in a new tab.

---

### Finding your way around the JupyterLab interface

Let's now spend a few minutes exploring the JupyterLab interface using the [documentation provided by JupyterLab](https://jupyterlab.readthedocs.io/en/stable/user/interface.html) to get started. Open the link in a fresh browser window and then return here when you have worked through the documentation there. Don't worry about covering all the details there. Just work out how we can use the Notebook terminal together with the added benefits of having a graphical user interface to work with files. Don't worry  

Fundamentally, your screen is divided into a few areas. You'll see context menus at the top (File, Edit, View, Run etc.), a file browser pane on the left, and an activity area that initially displays a launcher interface with tiles. Clicking on one of these tiles will open a new tab in the activity area.

---


### The Terminal
The terminal allows us to access a system shell (just like on ypur MacBooks), so let's start there.

Select File -> New -> Terminal, or click the Terminal icon on the launcher pane. You'll get a new terminal tab in the activity bar, and find yourself in a bash shell.

---


### Who is jovyan?

Looking at your bash prompt, you’ll notice that your username is **jovyan** (derived from "Jupyter"). Why does everyone have the same username in this environment? That’s because your notebook server is running inside a **container**. Containers are lightweight, self-contained environments that bundle an application (in this case, the Jupyter notebook server) together with its dependencies, ensuring that it runs consistently across different systems. 

The **container** instance is private and linked to your specific user storage on the system, but the actual image (which includes the software, configurations, and libraries) is the same for everyone using the platform. This is why it’s unnecessary to have unique system users for each individual—everyone operates within the same standardized environment, hence the shared username.

### What is a container?

A **container** is a technology that allows you to package up an application, along with all of its dependencies, libraries, and configurations, into a single isolated environment. This guarantees that the application runs consistently, no matter where it’s deployed. Unlike virtual machines, which simulate an entire operating system, containers are more lightweight because they share the host system’s kernel, making them faster and less resource-intensive.

In a Jupyter Notebook server context, containers allow multiple users to have their own private instances, even though they all use the same underlying software image. This ensures consistency across users, simplifies deployment, and provides isolation—what happens in your container doesn’t affect others. It's a key technology in environments where reproducibility and ease of scaling are important, like in data science and bioinformatics workflows.

**TLDR:** Don’t worry about it! Inside your notebook server, everyone’s username is set to **jovyan** because you’re working inside a standardized container environment that provides all the necessary tools and libraries for your Jupyter notebooks.

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

### Installing Programs in Conda Environments

**Bioconda** is a distribution of bioinformatics software for the Conda package manager. Conda is an open-source package manager that can install software and manage environments. While Conda is general-purpose and can manage software written in any language, Bioconda specifically targets the bioinformatics community, with a collection of over 7,000 bioinformatics packages, making it one of the largest and most comprehensive software distributions available for computational biology.

A straightforward `bioconda` tool that can calculate the GC percentage for sequences is `seqkit`, which provides a variety of functionalities for sequence file manipulations. Among its many utilities, it has a subcommand to compute the GC content.

To check if the program `seqkit` is available in `bioonda`, and which versions:

```bash
conda search -c bioconda seqkit
```


To install a package, simply replace `search` with `install`. If you also add `-y` you will not be prompted and will try to install directly.

```bash
conda install -y -c bioconda seqkit
```

**Demonstrate GC Percentage Calculation**

- First, create a sample FASTA file:

```bash
echo ">sample_sequence" > sample.fasta
echo "AGCTAGCTAGCTAGCTA" >> sample.fasta
```

- Note that using `>>` means the text is added to the end of the existing file. If we typed `>` the current file would be overwritten.

- Now, compute the GC content:

```bash
seqkit fx2tab sample.fasta --gc
```

This command will give you the GC content of the sequence in sample.fasta.

NB: `seqkit` is versatile and offers many other sequence-related utilities, so it's not just limited to computing the GC content. But for the sake of simplicity and demonstration to beginners, the GC content computation serves as an easy-to-understand example.

---

### A quick analysis of an *E. coli* genome

Let's do a quick analysis of an *E. coli* genome with `seqkit`.

Let's download and analyse a real genome!

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

