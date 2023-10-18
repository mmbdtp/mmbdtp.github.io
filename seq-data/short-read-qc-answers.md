---
layout: page
---

Answers for [short-read-qc exercises]({{site.baseurl}}/modules/sequencing/short-read-qc/)

### Exercise 1: Run FASTQE

**Run FASTQE on the example data, pKP1-NDM-1_R1.fastq.gz and female_oral2.fastq.gz. One at a time. Which data has the better average quality?**

```bash
fastqe female_oral2.fastq.gz 
fastqe pKP1-NDM-1_R1.fastq.gz 
```

![Alt text](image-2.png)

pKP1-NDM-1_R1.fastq.gz has the better quality. 

**Which portion of read has the lower average quality, and for which file?**

The tail end (3') of female_oral2.fastq.gz has the lowest average quality.

**What is the lowest mean score in female_oral2.fastq.gz?**

The lowest score in this dataset is 😿 13.




[Back to Programme]({{site.baseurl}}/modules/sequencing/week-2-programme/).
