---
title: Thursday - Session 3

---

# Introduction to Conda/Mamba

## Getting Started with Conda/Mamba

> **Things covered here:**
> - What is Conda/Mamba?
> - Why should I use a package and environment management system as part of my research workflow?
> - Why use Conda (Mamba)?
> How to install Mamba?

### Packages and Environments

#### Packages :package:

When working with a programming language, such as Python, that can do almost *anything*, one has to wonder how this is possible. You download Python, it has about 25 MB, how can everthing be included in this small data package. The answer is - it is not. Python, as well as many other programming languages use external libraries or packages for being able to doing almost anything. You can see this already when you start programming. After learning some very basics, you often learn how to *import* something into your script or session.

#### Dependencies :link:

A bit further into your programming career you may notice/have noticed that many packages do not just do everything on their own. Instead, they *depend* on other packages for their functionality. For example, the [Scipy](https://scipy.org/about/) package is used for numerical routines. To not reinvent the wheel, the package makes use of other packages, such as numpy (numerical python) and matplotlib (plotting) and many more. So we say that numpy and matplotlib are *dependencies* of Scipy.

Many packages are being further developed all the time, generating different *versions* of packages. During development it may happen that a function call changes and/or functionalities are added or removed. If one package can depend on another, this may create issues. Therefore it is not only important to know that e.g. Scipy depends on numpy and matplotlib, but also that it depends on numpy version >= 1.6 and matplotlib version >= 1.1. Numpy version 1.5 in this case would not be sufficient.

#### Environments :card_index_dividers:

When starting with programming we may not use many packages yet and the installation may be straightforward. But for most people, there comes a time when one version of a package or also the programming language is not enough anymore. You may find an older tool that depends on an older version of your programming language (e.g. Pyhton 2.7), but many of your other tools depend on a newer version (e.g. Python 3.6). You could now start up another computer or virtual machine to run the other version of the programming language, but this is not very handy, since you may want to use the tools together in a workflow later on. Here, *environments* are one solution to the problem. Nowadays there are several environment management systems following a similar idea: Instead of having to use multiple computers or virtual machines to run different versions of the same package, you can install packages in isolated environments.

#### Environment management :gear:

An environment management system solves a number of problems commonly encountered by scientists.

- An application you need for a research project requires different versions of your base programming language or different versions of various third-party packages from the versions that you are currently using.
- An application you developed as part of a previous research project that worked fine on your system six months ago now no longer works.
Code that was written for a joint research project works on your machine but not on your collaborators’ machines.
- An application that you are developing on your local machine doesn’t provide the same results when run on your remote cluster.

An environment management system enables you to set up a new, project specific software environment containing specific Python versions as well as the versions of additional packages and required dependencies that are all mutually compatible.

- Environment management systems help resolve dependency issues by allowing you to use different versions of a package for different projects.
- Make your projects self-contained and reproducible by capturing all package dependencies in a single requirements file.
- Allow you to install packages on a host on which you do not have admin privileges.

#### Package management :wrench:

A good package management system greatly simplifies the process of installing software by…

1. identifying and installing compatible versions of software and all required dependencies.
2. handling the process of updating software as more recent versions become available.

### Why should I use a package and environment management system?

Installing software is hard. Installing scientific software is often even more challenging. In order to minimize the burden of installing and updating software (data) scientists often install software packages that they need for their various projects system-wide.

Installing software system-wide has a number of drawbacks:

- It can be difficult to figure out what software is required for any particular research project.
- It is often impossible to install different versions of the same software package at the same time.
- Updating software required for one project can often “break” the software installed for another project.

Put differently, installing software system-wide creates complex dependencies between your research projects that shouldn’t really exist!

Rather than installing software system-wide, wouldn’t it be great if we could install software separately for each research project?

### Conda/Mamba

#### What is Conda?

Conda is an open source package and environment management system that runs on Windows, Mac OS and Linux.

- Conda can quickly install, run, and update packages and their dependencies.
- Conda can create, save, load, and switch between project specific software environments on your local computer.
- Although Conda was created for Python programs, Conda can package and distribute software for any language such as R, Ruby, Lua, Scala, Java, JavaScript, C, C++, FORTRAN.

Conda as a package manager helps you find and install packages. If you need a package that requires a different version of Python, you do not need to switch to a different environment manager, because Conda is also an environment manager. With just a few commands, you can set up a totally separate environment to run that different version of Python, while continuing to run your usual version of Python in your normal environment.

#### What is Mamba?

**Mamba is a reimplementation of conda** that offers the same functionality but with significant performance improvements. Written in C++, Mamba solves package dependencies much faster than conda, which becomes especially noticeable when working with complex environments or large package installations. It is a "drop-in" replacement for conda. meaning that most commands where you's use `conda` you can use mamba instead(e.g. `mamba install` instead of `conda install`). 

### Conda Channels and Licensing Considerations

#### What are Conda channels?

**`defaults` (Anaconda-managed):**
- Includes `main` and `r` repositories from Anaconda Inc.
- **Requires paid license** for organisations with 200+ employees (including research institutes)
- We cannot use this channel at NBI

**`conda-forge`:**
- Community-led, completely open source and free
- 20000+ packages available
- No licensing restrictions for any organization size

**`bioconda`:**
- Specialises in bioinformatics software (9000+ packages)
- Built on top of conda-forge
- Fully open source with no licensing restrictions

#### Our Setup: Miniforge with Mamba

At NBI, we need to comply with Anaconda's licensing requirements while maintaining access to all the bioinformatics and scientific computing tools we need. Therefore our setup is the following:

1. Use **Miniforge** instead of Anaconda/Miniconda (pre-configured for open source channels)
2. Use **Mamba** instead of Conda for faster package installation
3. Download packages from **Prefix.dev mirrors** instead of Anaconda servers

### Installation and Configuration


#### For a local installation (For example macOS)

**Step 1: Install Miniforge**

Download and install Miniforge from: [https://github.com/conda-forge/miniforge](https://github.com/conda-forge/miniforge)

For macOS:

```bash
wget "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh
```

Follow the installation prompts. When asked if you want to initialise Miniforge3, answer "yes". This will modify your shell configuration file (e.g., `~/.bashrc` or `~/.zshrc`) to make conda/mamba commands available.

After installation, restart your terminal or run:

```bash
source ~/.bashrc 

# or if using zsh
source ~/.zshrc 
```

**Step 2: Add the bioconda channel**

```bash
conda config --add channels bioconda
```

**Step 3: Switch to the Prefix channel mirror for open-source channels**

```bash
conda config --set channel_alias "https://repo.prefix.dev"
```

**Step 4: Verify your configuration**

```bash
conda config --show channels

channels:
  - bioconda
  - conda-forge


conda config --show channel_alias

channel_alias: https://repo.prefix.dev
```

<blockquote> 
<details><summary><strong>HPC Installation: Key differences for NBI HPC users</strong></summary>

<p>In this tutorial, we are walking through a mamba installation locally, however there are some slight differences between a local installation and installing on the NBI HPC:</p>

<ul>
<li>Installing without <code>conda init</code> to keep your <code>.bashrc</code> clean</li>
<li>Using <code>source activate</code> and <code>source deactivate</code> instead of <code>mamba activate</code> and <code>mamba deactivate</code></li>
</ul>

<p>For the HPC installation, replace Step 1 above with the following:</p>

<p><strong>Step 1:</strong></p>

<pre><code>wget "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-Linux-x86_64.sh -b -p ~/mamba
rm Miniforge3-$(uname)-$(uname -m).sh
ln -s ~/mamba/bin/activate ~/mamba/condabin/activate
ln -s ~/mamba/bin/deactivate ~/mamba/condabin/deactivate
export PATH=$PATH:/hpc-home/$USER/mamba/condabin
echo "export PATH=\$PATH:/hpc-home/$USER/mamba/condabin" >> ~/.bash_profile
</code></pre>

<p>These commands will:</p>

<ul>
<li>download the Mamba installation script</li>
<li>install Mamba into <code>~/mamba</code> without enabling conda init</li>
<li>delete the installation script (you will no longer need it)</li>
<li>make the mamba, activate and deactivate commands accessible to your shell both right now, and every time <code>~/.bashrc</code> is sourced</li>
</ul>

<p>Not enabling <code>conda init</code> is good practice in HPC environments - keeping your <code>~/.bashrc</code> file free of additions you didn't manually make yourself can prevent unexpected and difficult-to-diagnose issues with personal environments interfering with software execution (interactively or via Slurm).</p>

<p>The main consequence of the lack of <code>conda init</code> is that <code>mamba activate</code> and <code>mamba deactivate</code>, the most widely-used commands to activate Mamba environments (which you may see in other online tutorials/instructions), will not work. Instead, you will use <code>source activate</code> and <code>source deactivate</code>, which do the same thing.</p> <p>Throughout the rest of this tutorial, whenever you see <code>mamba activate</code> or <code>mamba deactivate</code>, HPC users should use <code>source activate</code> and <code>source deactivate</code> instead.</p>

<p><strong>Why the difference from HPC?</strong></p>

<p>On your local machine, enabling conda init is convenient and won't interfere with other systems. On the HPC, keeping <code>~/.bashrc</code> clean prevents conflicts with cluster-wide software modules.</p>

<p><strong>Steps 2-4:</strong></p>
<p>Follow the same configuration steps as above (adding bioconda channel, setting channel alias, and verifying configuration).</p>


</details>
</blockquote>

### Our first mamba environment

> **Note:** 
> Avoid installing packages into your base Mamba environment
> Mamba has a default environment called `base` that include a Python installation and some core system libraries and dependencies of Mamba. It is a “best practice” to avoid installing additional packages into your base software environment. Additional packages needed for a new project should always be installed into a newly created Mamba environment.


A Mamba environment is a directory that contains a specific collection of packages that you have installed. For example, you may be working on a research project that requires samtools version 1.22 and its dependencies, while another environment associated with a finished project has samtools 1.6 (perhaps because version 1.6 was the most current version of samtools at the time the project finished). If you change one environment, your other environments are not affected. You can easily activate or deactivate environments, which is how you switch between them.

For a list of all commands, take a look at [Conda general commands](https://docs.conda.io/projects/conda/en/latest/commands/index.html) (remember wherever you can use `conda` you can replace it with `mamba`).

#### Searching for available packages

> Always specify a version number for each package you wish to install
> In order to make your results more reproducible and to make it easier for research colleagues to recreate your Mamba environments on their machines it is a “best practice” to always explicitly specify the version number for each package that you install into an environment. If you are not sure exactly which version of a package you want to use, then you can use search to see what versions are available using the `mamba search` command.

> ```bash
> mamba search samtools
> ```


#### Creating and activating environments

```bash
# Lets create an environment with samtools v1.22 and check the version after install
mamba create --name samtools-1.22-env samtools=1.22
mamba activate samtools-1.22-env

samtools --version

# Let's deactivate before creating and activating the next environment
mamba deactivate

# Lets create an environment with samtools v1.6 and check the version after install
mamba create --name samtools-1.6-env samtools=1.6
mamba activate samtools-1.6-env

samtools --version

mamba deactivate
```

When we activate an environment, we can see the currently active environment in brackets like this:

```bash
(samtools-1.6-env) username@laptop
```

#### How environments relate to PATH

Remember when we learned about the `PATH` variable? When you activate a Mamba environment, Mamba temporarily modifies your `PATH` to point to the binaries in that specific environment. Let's see this in action:

```bash
# Check PATH before activating an environment
echo $PATH  | tr ":" "\n"

# Activate an environment and check PATH again
mamba activate samtools-1.22-env
echo $PATH  | tr ":" "\n"
mamba deactivate
```

Notice that the environment's bin directory is now at the front of `PATH`! This is how Mamba ensures you're using the correct version of each tool: by controlling which directories are searched first in your `PATH`!

We can check what environments we have installed and their locations (which get appended to `PATH`) by running the `mamba env list` command:

```bash

mamba env list
```
<br>

```bash
base                   /Users/<USERNAME>/miniforge3
samtools-1.22-env      /Users/<USERNAME>/miniforge3/envs/samtools-1.22-env
samtools-1.6-env       /Users/<USERNAME>/miniforge3/envs/samtools-1.6-env
```

Each environment is a separate directory, and when you activate one, Mamba adds that environment's `bin` subdirectory to the front of your `PATH`.

#### Installing multiple packages in the same environment

You could also create an environment and install multiple packages by listing the packages that you wish to install. 
We can either create a new environment or we can modify an existing environment. Let's try both ways.

We can always check what packages are installed in our current environment using the `mamba list` command after activating our environment

```bash
# Let's find our tool that we want to install
mamba search bcftools

# Let's create a new environment with both tools
mamba create --name test-env samtools=1.22 bcftools=1.22

mamba activate test-env
mamba list
mamba deactivate
```

We could also install the same tool into one of our existing environments, by activating the environment and then using the `mamba install` command:

```bash
mamba activate samtools-1.6-env
mamba list

# Let's add bcftools version 1.22 into this environment
mamba install bcftools=1.22

# check that it was succesful
mamba list
```

### Sharing environments from YAML files

For more complex projects or when sharing your work with collaborators, it's best practice to define your environment in a YAML file. This makes your computational environment fully reproducible and easy to share.

Let's export one of the environments we already created to see what a YAML file looks like:

```bash
# Activate the test-env we created earlier
mamba activate test-env

# Export to a YAML file
mamba env export > test-env.yml

# View the exported file
cat test-env.yml
```
Notice that the YAML file includes not just the tools you explicitly installed (samtools and bcftools), but also all their dependencies! Let's move to CLIMB notebooks and see how we would recreate this environment from this YAML file. 

Once logged into CLIMB, import the yaml file and simply run:

```bash
mamba env create -f test-env.yml

mamba env list
```

#### Acknowledgements

This training course was adapted from the [Carpentries Introduction to Conda for (Data) Scientists Course](https://carpentries-incubator.github.io/introduction-to-conda-for-data-scientists/index.html).