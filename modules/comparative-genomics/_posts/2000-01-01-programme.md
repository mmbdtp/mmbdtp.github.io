---
title: Comparative Genomics
---

# Comparative Genomics 
  
### Instructors and assistants:
- Dr Gemma Langridge (lead): Gemma.Langridge@quadram.ac.uk
- Dr Emma Waters
- Alice Nisbet
- Eleanor Hayles 

**Venue:** UB44A

**Required software**
To be installed in new Conda environment compgen
```
conda create --name compgen
conda activate compgen
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict
conda install prokka
conda install socru
conda install breseq
conda install refseq_masher
conda install mlst
conda install snippy
conda install sistr_cmd
conda install shovill
conda install roary
conda install artemis
conda install scoary

```

***

### Monday 
**Topic:** _Mapping and variant calling_  

**Dataset:** _A set of Salmonella enterica serovar Typhi samples that have been sequenced_

**Tasks:** 

- Compare the results of using short and long read sequencing to map to a reference. 
- What is similar, what is different and what impact does this have on the overall interpretation?
- What isn’t included when you do analysis using a reference genome?  
- Is it important and what does it do? 
- How do you choose a reference to use?  

**Schedule:**  

9:00-10:00 – Introduction and setup of task; installation of software  
10:00-12:00 – Task    
13:00- 15:00 - Task  
15:00-16:00 – Talk from Dr Claire Jenkins, UKHSA  
16:00-17:00 – Wrap up  
 
***

### Tuesday 
**Topic:** _Pan-genomes and mobile genetic elements_ 

**Tasks:**

- Construct a pan-genome of the dataset from Monday. 
- What is core to all of the genomes and what is accessory. 
- What is actually in the accessory genome, what do the genes do and why does it matter? 
- How do you tell between a mis-assembly, an integron, a plasmid etc... 

**Schedule:**  

9:00-10:00 – Introduction and setup of task  
10:00-12:00 – Task  
13:00-15:00 - Task  
15:00-16:00 – Wrap up   

***

### Wednesday 
**Topic:**_Genome comparison_ 

**Tasks:**

- Can you identify and visualise large structural variation?
- Why is this important?  
- Can you identify large insertions and deletions?
- What genes are impacted?
- Can you distinguish between an informatics artefact and a real signal? 

**Schedule:**  

9:00-10:00 – Introduction and setup of task  
10:00-12:00 – Task  
13:00-15:00 - Task  
15:00-15:30 – Talk from Dr Emma Waters   
15:30-16:30 – Wrap up  

***

### Thursday  - Free

***
 
### Friday 

**Topic:**_Outbreak investigation_  

**Dataset:** 

| biosample_acc | strain       | genBankAssembly | SRArun_acc |
|---------------|--------------|-----------------|------------|
| SAMN00860590  | CFSAN000191  | GCA_000698635.1 | SRR498369  |
| SAMN00862340  | CFSAN000228  | GCA_000748565.1 | SRR500493  |
| SAMN00862341  | CFSAN000212  | GCA_000748245.1 | SRR500494  |
| SAMN00989085  | CFSAN000211  | GCA_000698715.1 | SRR498373  |
| SAMN00991042  | CFSAN000661  | GCA_000698515.1 | SRR498397  |
| SAMN00991044  | CFSAN000669  | GCA_000749005.1 | SRR498399  |
| SAMN00991046  | CFSAN000700  | GCA_000749045.1 | SRR498402  |
| SAMN00991047  | CFSAN000752  | GCA_000749065.1 | SRR498403  |
| SAMN00991048  | CFSAN000753  | GCA_000749085.1 | SRR498404  |
| SAMN01816351  | CFSAN001118  | GCA_000748065.1 | SRR1258443 |
| SAMN01816352  | CFSAN001140  | GCA_000748085.1 | SRR1258440 |
| SAMN01816358  | CFSAN001112  | GCA_000748025.1 | SRR1258439 |
| SAMN01816359  | CFSAN001115  | GCA_000748045.1 | SRR1258442 |
| SAMN01823701  | CFSAN000189  | GCA_000439415.1 | SRR498276  |
| SAMN01942233  | CFSAN000951  | GCA_000749145.1 | SRR498422  |
| SAMN01942244  | CFSAN000954  | GCA_000748405.1 | SRR498425  |
| SAMN01942246  | CFSAN000963  | GCA_000749295.1 | SRR498436  |
| SAMN01942253  | CFSAN000961  | GCA_000748525.1 | SRR498434  |
| SAMN01942254  | CFSAN000960  | GCA_000748505.1 | SRR498433  |
| SAMN01942268  | CFSAN000968  | GCA_000698535.1 | SRR498442  |
| SAMN01942296  | CFSAN000970  | GCA_000749415.1 | SRR498444  |
| SAMN01942300  | CFSAN000958  | GCA_000748465.1 | SRR498431  |
| SAMN01942313  | CFSAN000952  | GCA_000749165.1 | SRR498423  |


**Tasks:** 

- This is a dataset from a real outbreak investigation. Time is critical. 
- Are the samples linked? 
- What information can you find out about these samples to aid the epidemiologists?  

**Schedule:**  

9:00-9:15 – Introduction and setup of task  
9:15-11:30 – Task  
11:30-12:30 – Wrap up  
 

