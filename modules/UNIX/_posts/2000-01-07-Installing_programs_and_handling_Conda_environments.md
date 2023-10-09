---
title: Installing programs and handling Conda environments
---

## Installing programs and handling Conda environments

In this tutorial, we will start with the basics of installing programs on the command line, focusing on MacOS, and then move on to more advanced topics like setting up Miniconda and Conda environments.


### Setup your Mac: Install Rosetta
`Rosetta` is a translation layer that enables applications compiled for Intel-based Macs to run on Apple Silicon Macs, such as those using the M1 or M2 chip. Without Rosetta, applications that have not been compiled natively for Apple Silicon may not run.

However, macOS comes with a number of built-in Unix commands, and these commands are native to the operating system. Therefore, most standard Unix commands provided by macOS itself (like ls, cd, mv, etc.) are unaffected by the presence or absence of Rosetta, as they will have native versions for the architecture.

The commands that might be affected if Rosetta is not installed are those from third-party software packages which have been compiled for Intel architecture and have not yet been updated for Apple Silicon. This could include certain package managers and the software they distribute, or specialized command-line tools.

For example, if you're using Homebrew (we'll take about this in a minute or two)  and install a package that hasn't been compiled for Apple Silicon, without Rosetta that package/tool might not run. Similarly, if you rely on certain tools or binaries that haven't been updated for the M1 architecture, those might be problematic without Rosetta.

Your MacBooks have M2 chips, so you do now need to install [Rosetta](https://support.apple.com/en-gb/HT211861).


Open a terminal and type:

```bash
softwareupdate --install-rosetta --agree-to-license
```

Now we can configure our Terminal to be executed under rosetta, which can make our life easier:

1. Locate the Terminal application in the Finder
2. Right-click and select `Get Info`
3. Tick the checkbox `Open using Rosetta`

---

### Install the command line tools

A lot of advanced tools benefit from the presence of a compiler, to build programs from source. 

From the terminal, type:

```bash
xcode-select --install
```

![Screenshot](https://www.ics.uci.edu/~pattis/common/handouts/macclion/images/clang/Clang%20xcode-select.png)



###Installing Programs using Homebrew
**Homebrew** is a popular package manager for MacOS which simplifies the installation of software. Let's get started by installing Homebrew.


Enter the following command to install `Homebrew`:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Then follow the on-screen instructions.

Once you have `Homebrew` installed, you can use it to install various software packages. For example, let's install `wget`, a commonly used tool for downloading files over the web, with some advantages over `curl`.

```
brew install wget
```

Try using `wget` to get a sequence file from GenBank.

```
#make sure you are in your home directory
cd ~
wget "https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000005845.2/download?include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT&filename=GCF_000005845.2.zip" -O coligenome.zip
```

`wget` saves the file as default, but we want to give it a simpler name. The -O option allows us to specify what we want the output file to be called, But take care! Unix is case-sensitive so it has to be an upper-case "O". Most flags are case sensitive. Take a look with `ls`. 

Note the file extension: `.zip`. ZIP files are a popular archive file format that compresses one or more files or folders into a single file for easier storage and transport. They not only reduce the overall size of the files but also bundle multiple files and directories into a single entity. The .zip extension typically identifies ZIP files. 

macOS comes with a built-in command-line utility to handle ZIP files. To unzip a file from the Terminal, you can use the `unzip` command followed by the filename.


```
unzip coligenome.zip
```

This will extract the contents of `coligenome.zip `into the current directory. Take another look at the directory with `ls`.

The `Homebrew` approach is okay if we want to install simple programs for simple purposes. But if we want to do complex bioinformatics work, we need to use a more complex package manager. 

Let's talk about **Conda**. 

---

###Conda

**Conda** is an open-source, cross-platform, language-agnostic package manager and environment management system. 

While both `Conda` and `Homebrew` are package managers, they serve slightly different purposes:

* **Environment Management:** `Conda` excels in creating isolated environments, allowing you to have different versions of software (and even Python itself) without conflicts. This is especially useful for scientific projects that might have varying dependencies.
* **Cross-Platform:** While `Homebrew` is primarily for macOS (though there's a version for Linux), `Conda` works on macOS, Linux, and Windows. This consistency can be beneficial for collaborative projects across different operating systems.
* **Comprehensive**: `Conda` is not just a package manager but also an environment manager. It’s designed specifically for scientific computing and data science workflows, making it easier to manage complex dependencies in these domains.
* **Unified Packages**: With `Conda`, both the language-specific packages (like Python or R packages) and system-level libraries can be managed in a unified manner, reducing the complexity of installations.

In summary, while `Homebrew` is excellent for general software installations on macOS, conda offers a more specialized environment and package management system tailored to bioinformatics and data science tasks. You may or may not want to use conda on your MacBook, but you will definitely need to use it on the notebook servers we are using later in the course, which you are also likely to use in your research.

`Miniconda` is a cut-down version of `Conda` that includes only the `Conda` system and its dependencies. Miniconda was developed to simplify the installation of Python tools and the creation of isolated environments (to allow, for example, the insulation of conflicting packages). Miniconda quickly became a fantastic solution to the problem providing:

* a package manager that runs in the user space (not requiring sudo privilege)
* an easy way to add new packages to the repository (in fact, repositories)


**But what's the difference between Conda and Miniconda?**

Imagine you're buying a new smartphone.

* **Conda** is like getting a smartphone with all the fancy apps pre-installed. It's great because you have everything from the get-go, but it might have some apps you'll never use. Conda comes with a lot of pre-installed packages, making the initial download larger.
* **Miniconda** is like getting just the smartphone with only the essential apps. It's lightweight and fast to start with. You only download and add the apps (or packages) you need, when you need them. Miniconda gives you the `Conda` tool itself and lets you choose your packages as you go.
So, both give you a smartphone (or the `Conda` system). But `Conda` gives you all the bells and whistles from the start, while` Miniconda` lets you pick and choose as you go along.

Today we are going to use `Miniconda`. 

**Installing Miniconda**

The latest version is available from the [offical website](https://docs.conda.io/en/latest/miniconda.html). There are installers there for macOS and Linux and versions based on different versions of Python. If you want to install miniconda somehwere, always use the most appropriate link for your platform and the version desired (avoid Python 2.7 and 32-bit versions)

But to get started on your M@ MacBooks, here a typical workflow, that will install Miniconda in your home directory.

First, download the Miniconda installer for MacOS:

```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
```
Give the script installer permissions

```
chmod +x Miniconda3-latest-MacOSX-arm64.sh
```

Then run the installer script:

```
./Miniconda3-latest-MacOSX-arm64.sh
```

This will start an interactive process that will ask some questions (to accept the license and to start the initializer). Follow the on-screen instructions.

When the process is finished you need to restart your shell i. e. to log out and login again

```
bash
zsh
```

Check that Miniconda is properly initialized by asking for the `Conda` Version:

```
conda --version
```

If Miniconda is properly installed and initialized, this command will display the version of conda that you have installed.

You should also be able to see that you are working within Miniconda's base environment as your prompt line will now have the term `(base)` at the start.

---

###Setting Up Conda Environments

Conda simplifies installing packages, but a problem remains: conflicting versions. 

* You may want to use `samtools 1.10`, for example, but another tool installed an older version because it’s not yet ready to support a more recent one. 
* A typical bioinformatics workflow involves dozens of different tools, sometimes each requiring a broad range of libraries and other dependencies. 
* Installing all of them is a tedious task and sometimes an impossible task when different packages require different versions of the same tool. 


There are two main solutions to the problem: 

- containers, which resolve the problem of conflicting packages, but does not necessarily simplify the installation of the packages) 
- package managers like Conda that allow us to create and manage environments.

A **Conda environment** is an isolated directory that contains a specific collection of software that you can activate or deactivate. This helps you maintain different versions of software and prevents conflicts. 

*But, really, why bother?? Can you explain all this in simple terms*

Think of Conda environments like separate, tidy little boxes where you can store tools and toys without them getting mixed up! 📦 

* Imagine when you want to have fun with some kids drawing and painting, you have a box (environment) for drawing that has colored pencils, erasers, and sketchpads (software & packages). 
* You don't want your kids to get hold of paints (other software) to mix with them and create a mess, right? 🎨 
* So, you create different boxes (environments) for different hobbies (projects). With Conda environments, you do something similar but with software. 
* You create an isolated space (an environment) where you can keep the specific tools (software and packages) you need for a particular task or project, keeping everything neat and organized! 🧹 
* This way, you can easily switch between tasks (projects) without your tools (software) interfering with each other! 🔄 
* And if something goes wrong in one box (environment), it doesn’t affect the others, keeping them safe and sound! 🛡️ 

So, in a nutshell, Conda environments help you manage your software tools in a clean, organized, and project-specific manner! 🧠🎉

**Create a new environment**

We need to choose a unique name for our new environment, in this example myenv1 (usually it’s the name of a tool (like qiime2-2020.1) or a task (like denovo):

```
conda create -n myenv1
```
**Activate the environment**

To use an environment we need first to activate it:

```
conda activate myenv1
```
When the new environment is active, you will no longer be able to access any packages you installed in the base environment and if you install a package now it will belong to the active environment.

###Conda channels

Conda allows to install packages from a *default channel* (mainly containing python modules), but also supports third party channels. 

If we want conda to install bioinformatics programs, we need to tell it which channels to look at. There are three channels that can be of particular interest in (bio)data science:


```
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --add subdirs osx-64
#that last line is needed for new MacBooks!

```

###Installing Programs in Conda Environments

**Bioconda** is a distribution of bioinformatics software for the Conda package manager. Conda is an open-source package manager that can install software and manage environments. While Conda is general-purpose and can manage software written in any language, Bioconda specifically targets the bioinformatics community, with a collection of over 7,000 bioinformatics packages, making it one of the largest and most comprehensive software distributions available for computational biology.

A straightforward `bioconda` tool that can calculate the GC percentage for sequences is `seqkit`, which provides a variety of functionalities for sequence file manipulations. Among its many utilities, it has a subcommand to compute the GC content.

To check if the program `seqkit` is available in `bioonda`, and which versions:

```
conda search -c bioconda seqkit
```


To install a package, simply replace `search` with `install`. If you also add `-y` you will not be prompted and will try to install directly.

```
conda install -y -c bioconda seqkit
```

**Demonstrate GC Percentage Calculation**

- First, create a sample FASTA file:

```
echo ">sample_sequence" > sample.fasta
echo "AGCTAGCTAGCTAGCTA" >> sample.fasta
```

- Note that using `>>` means the text is added to the end of the existing file. If we typed `>` the current file would be overwritten.

- Now, compute the GC content:

```
seqkit fx2tab sample.fasta --gc
```

This command will give you the GC content of the sequence in sample.fasta.

NB: `seqkit` is versatile and offers many other sequence-related utilities, so it's not just limited to computing the GC content. But for the sake of simplicity and demonstration to beginners, the GC content computation serves as an easy-to-understand example.


**Deactivating an environment**

To deactivate the current environment and return to the previous environment:

```
conda deactivate
```

Now you are in the base environment, try using `seqkit` again

```
seqkit fx2tab sample.fasta --gc
```

It doesn't work!!!


**List your environments**

To get a list of the environments in your system:

```
conda info --envs
```
**Delete an environment**

To be used with care

```
conda remove -n ENVIRONMENT_NAME --all

```

---

That's all for today!

---

[Back to the Programme for this week](week_1__programme.md)
