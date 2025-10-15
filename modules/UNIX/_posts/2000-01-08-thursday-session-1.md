---
title: Thursday - Session 1
---

# Cloud Computing with CLIMB

>**Things covered here:**
>
> - Introduction to CLIMB
> - Exercises to recap Bash


Although your MacBooks run a version of Unix, most bioinformaticians prefer to use Linux and so most programs and pipelines work best on Linux. In bioinformatics, most researchers also prefer to use remote virtual servers over their own personal (bare metal) machines for several important reasons:
 - Bioinformatics analyses, such as genome assembly, sequence alignment, or large-scale data mining, can require substantial computational resources (e.g., CPU power, memory, and storage). 
- Personal computers, particularly laptops, often do not have the processing power or memory to handle these large datasets effectively. 
- By contrast, remote servers provide access to high-performance computing resources, which are specifically optimized for demanding tasks, ensuring faster and more efficient data processing. Here we are using a cloud platform, CLIMB-BIG-DATA, offering dynamic scalable resources.
- Virtualization allows multiple users to share the same physical hardware while operating in isolated environments, making it a more cost-effective solution. 
- Virtual servers can be easily scaled up or down based on demand, meaning researchers only pay for the computing power they use, which is especially beneficial when dealing with fluctuating project needs. Virtual servers also offer enhanced redundancy and fault tolerance, meaning that if hardware fails, another server can quickly take over without data loss or significant downtime. This reliability is crucial for long-running bioinformatics computations, which could otherwise be disrupted by hardware failures.

We will be using a Jupyter notebook server, which is an easy-to-access web-based platform that allows users to create and interact with Jupyter Notebooks, which in turn are documents that can contain live code, equations, visualizations, and text. The server runs the computational backend that provides command line access to a Linux shell, but can also processes code cells written in languages like Python, R, or Julia, and returns the output to the browser interface where users can view and manipulate results.

Before we get stuck in to using a Notebook server, let's set the scene with a short [Powerpoint presentation](https://github.com/mmbdtp/mmbdtp.github.io/blob/c33a7727fc90026d4c78733c77a1f5935aea9f74/modules/UNIX/_posts/20250707_CBS_CLIMB.pptx)

## How to launch and access a CLIMB BIG DATA notebook server

* You have been sent an email invitation that explains how to access the CLIMB-BIG_DATA account interface Bryn [https://bryn.climb.ac.uk/](https://bryn.climb.ac.uk/) and set up an account.
* Read this information about authentication:
  * [https://docs.climb.ac.uk/getting-started/authentication/](https://docs.climb.ac.uk/getting-started/authentication/)
* Using the navigation menu on the left hand side, select 'Notebook servers' under the 'Compute' subheading
* Click the 'Launch notebook server' green action button on the right hand side
* Select a 'Standard server'
* Click 'Launch Server' and monitor the progress bar
* Once ready, click the url beneath the 'User notebook server'
* On first login, you may be asked to authorize access to your Bryn account. Click 'Authorize'
* The JupyterLab interactive computing interface should open in a new tab.

## Tips for using CLIMB

- Explore the file system with the side bar or with standard bash commands
- Use the shared team folder to store you work not your home drive (if your home drive fills up you cannot log in and it cannot be expanded)

## The Terminal
The terminal allows us to access a system shell (just like on your MacBooks), so let's start there.

Select File -> New -> Terminal, or click the Terminal icon on the launcher pane. You'll get a new terminal tab in the activity bar, and find yourself in a bash shell.



## Who is jovyan?

Looking at your bash prompt, you’ll notice that your username is **jovyan** (derived from "Jupyter"). Why does everyone have the same username in this environment? That’s because your notebook server is running inside a **container**. Containers are lightweight, self-contained environments that bundle an application (in this case, the Jupyter notebook server) together with its dependencies, ensuring that it runs consistently across different systems. 

The **container** instance is private and linked to your specific user storage on the system, but the actual image (which includes the software, configurations, and libraries) is the same for everyone using the platform. This is why it’s unnecessary to have unique system users for each individual—everyone operates within the same standardized environment, hence the shared username.


## What is a container?

A **container** is a technology that allows you to package up an application, along with all of its dependencies, libraries, and configurations, into a single isolated environment. This guarantees that the application runs consistently, no matter where it’s deployed. Unlike virtual machines, which simulate an entire operating system, containers are more lightweight because they share the host system’s kernel, making them faster and less resource-intensive.

In a Jupyter Notebook server context, containers allow multiple users to have their own private instances, even though they all use the same underlying software image. This ensures consistency across users, simplifies deployment, and provides isolation—what happens in your container doesn’t affect others. It's a key technology in environments where reproducibility and ease of scaling are important, like in data science and bioinformatics workflows.

**TLDR:** Don’t worry about it! Inside your notebook server, everyone’s username is set to **jovyan** because you’re working inside a standardized container environment that provides all the necessary tools and libraries for your Jupyter notebooks.


## Where am I? Who am I?

By default, you're in a bash shell running against the base operating system of the climb-jupyterhub container image (which is based on a flavour of Linux called Ubuntu). You'll see in your bash prompt that you're in your home directory (represented by the tilde character ~).


```
jovyan:~$ pwd
```

```
/home/jovyan
```

## There is no `sudo` here

One Unix command that you cannot use on this Notebook server is `sudo`. Try it
```
sudo touch file.txt
```
`jovyan` doesn't have sudo privileges. This may seem restrictive, but we've pre-configured the climb-jupyter base image with everything you'd likely need sudo for pre-installed. Everything else should be installable via package managers, such as Conda, which we are going to learn more about after coffee. 


## Using the Notebook terminal

Within the Terminal here, you can do almost all the things we did yesterday using Unix on your MacBooks.

### Recap Tasks:

- In the terminal, create a for loop that outputs the name of any file that is detected as empty in the folder shared-team/2025_training/week1/unix_intro/gene_annotations

- Can you make it into a script that you can pass a folder of your choice to check for empty files?

- Can you make it run like a standard bash command? (i.e. without calling bash first with `bash <script>.sh`)

- How do you get bash to do resolve basic arithmetic (5 + 10)? (May have to ask google)

- Challenge: Find all the genes with a sequence length greater than 2000 nucleotides in gene_annotations.csv file. Output the full sequences of these genes to a file called long_genes.fa

### Acknowledgements
This training course was adapted from material develop by Andrea Telatin and Lisa Marchioretto.
