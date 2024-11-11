---
title: Monday Morning - Introducing and Profiling the Dataset
---

# Monday Morning
## Introducing and Profiling the Dataset

---

## Objectives
By the end of this session, you will be able to:

1. **Understand the Importance of Profiling in Metagenomics**:
   - Comprehend the relevance of metagenomic profiling in analyzing and interpreting microbial community structures.
   - Relate the clinical and biological significance of data obtained from patient samples to potential diagnostic and treatment insights.

2. **Navigate and Use the NCBI SRA Database**:
   - Access and explore BioProjects, BioSamples, and Sequence Read Runs (SRR) on the NCBI platform.
   - Understand how to interpret metadata associated with metagenomic sequencing datasets.

3. **Execute Metagenomic Profiling Using Kraken2 and Krona**:
   - Run the `kraken2` tool to classify metagenomic sequences and generate detailed taxonomic reports.
   - Visualize the taxonomic output using Krona plots to interpret the diversity and composition of microbial communities.

4. **Develop and Use Shell Scripts for Batch Processing**:
   - Understand and adapt a shell script to analyze multiple paired-end sequencing files in a batch.
   - Enhance command-line scripting skills by modifying and troubleshooting scripts for specific needs.

5. **Generate and Interpret MultiQC Reports**:
   - Combine results from multiple samples using `multiqc` to obtain an integrated report.
   - Interpret MultiQC outputs to assess overall data quality and compare taxonomic classifications across samples.

6. **Critically Analyze and Compare Results**:
   - Compare the outputs from the generated plots and reports with existing NCBI analyses.
   - Discuss the potential discrepancies or limitations of locally generated results versus published data.

7. **Apply Bioinformatics Tools to Real-World Clinical Data**:
   - Utilize provided clinical metadata to draw connections between patient conditions and metagenomic profiles.
   - Interpret clinical metadata and correlate it with microbiome findings to understand potential impacts on patient health.

8. **Troubleshoot Common Issues in Metagenomic Workflows**:
    - Identify common challenges that may arise when using `kraken2`, `krona`, and related tools, and apply solutions to resolve them.
    - Discuss strategies for efficient data handling and analysis when working with large metagenomic datasets.

---

## Activate conda environment 

Let's start by cleaning up and configuring Conda

```
conda clean --all
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict


```
To save time we are going to use the `seqanalysis` environment we created earlier in the course.

```
conda activate seqanalysis
conda install multiqc metaphlan bowtie2 samtools
```

Let's create and use a directory in the shared-teams directory for these analyses (where `yourname` is replaced with your name!):

```
cd /home/jovyan/shared-team/2024_training/2024_metagenomics
mkdir metagenomics_2024_yourname
cd metagenomics_2024_yourname
```


---


## Powerpoint time
Now, we will provide the conceptual background to metagenomics with a brief [Powerpoint presentation](https://github.com/mmbdtp/mmbdtp.github.io/raw/refs/heads/gh-pages/modules/metagenomics/_posts/pallen-intro-metagenomics.pptx).


---


## The datasets

We will use human faecal metagenome samples from [a paper](https://www.microbiologyresearch.org/content/journal/mgen/10.1099/mgen.0.000293) from my research group on critically ill patients on an intensive care unit (ICU) in Birmingham. 
 
Here's a table, integrating clinical information and sequence data on the patient samples.

<table style="font-size: 11px; width: 100%; border-collapse: collapse;">
  <tr>
    <th style="border: 1px solid #ddd; padding: 8px;">Patient</th>
    <th style="border: 1px solid #ddd; padding: 8px;">Age</th>
    <th style="border: 1px solid #ddd; padding: 8px;">Sex</th>
    <th style="border: 1px solid #ddd; padding: 8px;">Clinical Features</th>
    <th style="border: 1px solid #ddd; padding: 8px;">ICU Day</th>
    <th style="border: 1px solid #ddd; padding: 8px;">Sample</th>
    <th style="border: 1px solid #ddd; padding: 8px;">Run</th>
    <th style="border: 1px solid #ddd; padding: 8px;">AvgSpotLen</th>
    <th style="border: 1px solid #ddd; padding: 8px;">Bases</th>
    <th style="border: 1px solid #ddd; padding: 8px;">Bytes</th>
    <th style="border: 1px solid #ddd; padding: 8px;">SOFA score</th>
    <th style="border: 1px solid #ddd; padding: 8px;">Antibiotics</th>
  </tr>
  <tr>
    <td style="border: 1px solid #ddd; padding: 8px;">Patient 02</td>
    <td style="border: 1px solid #ddd; padding: 8px;">64</td>
    <td style="border: 1px solid #ddd; padding: 8px;">F</td>
    <td style="border: 1px solid #ddd; padding: 8px;">Subarachnoid haemorrhage</td>
    <td style="border: 1px solid #ddd; padding: 8px;">1</td>
    <td style="border: 1px solid #ddd; padding: 8px;">13-6929524</td>
    <td style="border: 1px solid #ddd; padding: 8px;">SRR8926116</td>
    <td style="border: 1px solid #ddd; padding: 8px;">233</td>
    <td style="border: 1px solid #ddd; padding: 8px;">270,551,026</td>
    <td style="border: 1px solid #ddd; padding: 8px;">136,892,477</td>
    <td style="border: 1px solid #ddd; padding: 8px;">8</td>
    <td style="border: 1px solid #ddd; padding: 8px;"></td>
  </tr>
  <tr>
    <td style="border: 1px solid #ddd; padding: 8px;">Patient 02</td>
    <td style="border: 1px solid #ddd; padding: 8px;">64</td>
    <td style="border: 1px solid #ddd; padding: 8px;">F</td>
    <td style="border: 1px solid #ddd; padding: 8px;">Subarachnoid haemorrhage</td>
    <td style="border: 1px solid #ddd; padding: 8px;">10</td>
    <td style="border: 1px solid #ddd; padding: 8px;">13-6929534</td>
    <td style="border: 1px solid #ddd; padding: 8px;">SRR8926119</td>
    <td style="border: 1px solid #ddd; padding: 8px;">229</td>
    <td style="border: 1px solid #ddd; padding: 8px;">286,911,856</td>
    <td style="border: 1px solid #ddd; padding: 8px;">144,866,803</td>
    <td style="border: 1px solid #ddd; padding: 8px;">3</td>
    <td style="border: 1px solid #ddd; padding: 8px;"></td>
  </tr>
  <tr>
    <td style="border: 1px solid #ddd; padding: 8px;">Patient 04</td>
    <td style="border: 1px solid #ddd; padding: 8px;">75</td>
    <td style="border: 1px solid #ddd; padding: 8px;">M</td>
    <td style="border: 1px solid #ddd; padding: 8px;">Aortic aneurysm repair</td>
    <td style="border: 1px solid #ddd; padding: 8px;">10</td>
    <td style="border: 1px solid #ddd; padding: 8px;">13-6929537</td>
    <td style="border: 1px solid #ddd; padding: 8px;">SRR8926181</td>
    <td style="border: 1px solid #ddd; padding: 8px;">225</td>
    <td style="border: 1px solid #ddd; padding: 8px;">184,808,950</td>
    <td style="border: 1px solid #ddd; padding: 8px;">93,747,180</td>
    <td style="border: 1px solid #ddd; padding: 8px;">13</td>
    <td style="border: 1px solid #ddd; padding: 8px;">Flucoxacillin; Rofampicin; Erythromycin</td>
  </tr>
  <tr>
    <td style="border: 1px solid #ddd; padding: 8px;">Patient 04</td>
    <td style="border: 1px solid #ddd; padding: 8px;">75</td>
    <td style="border: 1px solid #ddd; padding: 8px;">M</td>
    <td style="border: 1px solid #ddd; padding: 8px;">Aortic aneurysm repair</td>
    <td style="border: 1px solid #ddd; padding: 8px;">14</td>
    <td style="border: 1px solid #ddd; padding: 8px;">13-6929548</td>
    <td style="border: 1px solid #ddd; padding: 8px;">SRR8926187</td>
    <td style="border: 1px solid #ddd; padding: 8px;">213</td>
    <td style="border: 1px solid #ddd; padding: 8px;">114,050,219</td>
    <td style="border: 1px solid #ddd; padding: 8px;">57,808,179</td>
    <td style="border: 1px solid #ddd; padding: 8px;">11</td>
    <td style="border: 1px solid #ddd; padding: 8px;">Flucoxacillin; Rofampicin; Meropenem</td>
  </tr>
  <tr>
    <td style="border: 1px solid #ddd; padding: 8px;">Patient 29</td>
    <td style="border: 1px solid #ddd; padding: 8px;">80</td>
    <td style="border: 1px solid #ddd; padding: 8px;">M</td>
    <td style="border: 1px solid #ddd; padding: 8px;">Subcapsular haematoma; liver cancer</td>
    <td style="border: 1px solid #ddd; padding: 8px;">15</td>
    <td style="border: 1px solid #ddd; padding: 8px;">13-6929598</td>
    <td style="border: 1px solid #ddd; padding: 8px;">SRR8926127</td>
    <td style="border: 1px solid #ddd; padding: 8px;">249</td>
    <td style="border: 1px solid #ddd; padding: 8px;">361,066,223</td>
    <td style="border: 1px solid #ddd; padding: 8px;">159,940,956</td>
    <td style="border: 1px solid #ddd; padding: 8px;">1</td>
    <td style="border: 1px solid #ddd; padding: 8px;">Meropenem</td>
  </tr>
  <tr>
    <td style="border: 1px solid #ddd; padding: 8px;">Patient 35</td>
    <td style="border: 1px solid #ddd; padding: 8px;">49</td>
    <td style="border: 1px solid #ddd; padding: 8px;">M</td>
    <td style="border: 1px solid #ddd; padding: 8px;">Lung transplant</td>
    <td style="border: 1px solid #ddd; padding: 8px;">3</td>
    <td style="border: 1px solid #ddd; padding: 8px;">13-6929625</td>
    <td style="border: 1px solid #ddd; padding: 8px;">SRR8926193</td>
    <td style="border: 1px solid #ddd; padding: 8px;">276</td>
    <td style="border: 1px solid #ddd; padding: 8px;">135,189,099</td>
    <td style="border: 1px solid #ddd; padding: 8px;">64,243,237</td>
    <td style="border: 1px solid #ddd; padding: 8px;">11</td>
    <td style="border: 1px solid #ddd; padding: 8px;"></td>
  </tr>
  <tr>
    <td style="border: 1px solid #ddd; padding: 8px;">Patient 36</td>
    <td style="border: 1px solid #ddd; padding: 8px;">30</td>
    <td style="border: 1px solid #ddd; padding: 8px;">M</td>
    <td style="border: 1px solid #ddd; padding: 8px;">Multiple trauma</td>
    <td style="border: 1px solid #ddd; padding: 8px;">11</td>
    <td style="border: 1px solid #ddd; padding: 8px;">13-6929614</td>
    <td style="border: 1px solid #ddd; padding: 8px;">SRR8926233</td>
    <td style="border: 1px solid #ddd; padding: 8px;">254</td>
    <td style="border: 1px solid #ddd; padding: 8px;">153,261,668</td>
    <td style="border: 1px solid #ddd; padding: 8px;">70,498,579</td>
    <td style="border: 1px solid #ddd; padding: 8px;">3</td>
    <td style="border: 1px solid #ddd; padding: 8px;">Co-Trimoxazole; Valganciclovir; Tazobactam</td>
  </tr>
</table>



The **SOFA (Sequential Organ Failure Assessment) score** is used to assess the extent of a patient's organ function or rate of failure. It helps predict outcomes and indicates the severity of illness, especially in intensive care units (ICU). The higher the score, the greater the likelihood of organ dysfunction and mortality risk. Here’s a quick interpretation:

- **Low scores (1-5)**: Typically suggest a lower risk of organ failure and better patient condition.
- **Moderate scores (6-10)**: Indicate a moderate level of organ dysfunction with an increased risk of adverse outcomes.
- **High scores (≥11)**: Represent significant organ dysfunction with a higher risk of mortality.

Based on scores for these patients:
- Scores of **1, 3, and 3** are low and generally indicate a better prognosis.
- Scores of **8 and 11** are in the moderate to high range, implying some level of concern.
- Scores of **13 and 11** are higher, suggesting more severe organ dysfunction and a higher risk.

Overall, we have a range of scores from low (1) to very concerning (13), indicating variability in patient conditions from relatively stable to high risk.

---


## Exploring the data and metadata

If you are interested in a dataset from a paper, it will be usually deposited to the [Sequence Read Archive](https://www.ncbi.nlm.nih.gov/sra) (SRA) at the National Center for Biotechnology Information (NCBI). The data are organised into BioProjects, BioSamples, SRA experiments and Runs. Take a look at the BioProjects and other components for the ICU study: [https://www.ncbi.nlm.nih.gov/bioproject/PRJNA533528](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA533528)

Explore the site's hierarchy of BioProjects, BioSamples, Sequence Read Experiments (SRX) and Sequence Read Runs (SRR). Review the NCBI metadata associated with each of the samples and runs we are going to be working on, using the links in the table above. Get Google Maps to show you where the samples were collected. Do you trust what it is telling you?

You can select a run and take a look at NCBI's phylogenetic analyses, including a Krona plot (click on *Show Krona View* to see it).
Here's the table with patient, day in ICU, and SRR link:

<div style="font-size: 12px;">
<table>
  <thead>
    <tr>
      <th>Patient</th>
      <th>ICU Day</th>
      <th>Run Link</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Patient 02</td>
      <td>1</td>
      <td><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926116&display=analysis">SRR8926116</a></td>
    </tr>
    <tr>
      <td>Patient 02</td>
      <td>10</td>
      <td><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926119&display=analysis">SRR8926119</a></td>
    </tr>
    <tr>
      <td>Patient 04</td>
      <td>10</td>
      <td><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926181&display=analysis">SRR8926181</a></td>
    </tr>
    <tr>
      <td>Patient 04</td>
      <td>14</td>
      <td><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926187&display=analysis">SRR8926187</a></td>
    </tr>
    <tr>
      <td>Patient 29</td>
      <td>15</td>
      <td><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926127&display=analysis">SRR8926127</a></td>
    </tr>
    <tr>
      <td>Patient 35</td>
      <td>3</td>
      <td><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926193&display=analysis">SRR8926193</a></td>
    </tr>
    <tr>
      <td>Patient 36</td>
      <td>11</td>
      <td><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926233&display=analysis">SRR8926233</a></td>
    </tr>
  </tbody>
</table>
</div>

Equipped with accession numbers you could use command line tools like`fastq-dump` from the [SRA Toolkit](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software) to retrieve FASTQ files. 

But to save time I have downloaded the files, renamed them with the metadata and made them available in

 -  `shared-team/2024_training/2024_metagenomics/sequences/`

I have also added some indexed files from the human genome. Copy that directory and files across into your current directory. 


```
cp -r  /home/jovyan/shared-team/2024_training/2024_metagenomics/sequences . 

```

---

## Coffee break
![](https://github.com/mmbdtp/mmbdtp.github.io/raw/refs/heads/gh-pages/modules/metagenomics/_posts/DALL·E%202024-11-11%2009.50.14%20.webp)
---


##  Profiling of reads


---

We would normally be running fastqc and trimmomatic over such sequences, but we are going to miss out those steps in the interest of time, particulaly as they probably were performed before the sequences were uploaded.


### Handling Kraken and krona

What we want to do now is run `kraken2` (with the `pluspf_8gb` database) and `krona` and then `multiqc` over all the trimmed sequence files

But `kraken2` won't accept wild cards, so you cannot just issue a single command like this:

```
kraken *.fastq | krona *report.txt | multiqc
```

You have several options:

 - patiently type in each command for all the sequences for kraken and wait until each is complete before running the krona command on the outputs.
 	- you could make each command run in the background by adding ` &` to the end
 - write a shell script that analyses all paired-end fastq files in a given directory
 	- you can do this yourself if you feel confident in loops and scripting
 - you can ask chatGPT to write you a shell script
 	- but you will have to optimise the prompt and may have to troubleshoot its performance
 	- and you should read ChatGPT's explanation of every line
 - you can use this script that I created using chatGPT, kraken_script.sh (see below)
 	- you will need to copy across into a file in your directory and grant it permissions to run
 	- only do this if your primary interest is scrutinising the biological significance of the results rather than learning how to do analyses
 - If you wrote your own script with or without help from ChatGPT, how did it differ from mine?


```
#!/bin/bash

# Create output directories if they don't exist
mkdir -p kraken_on_reads
mkdir -p krona_on_reads
mkdir -p multiqc_kraken_on_reads

# Loop through each R1 file in the directory
for r1_file in *_R1.fastq.gz; do
    # Extract the base name by removing the suffix
    base=$(basename "$r1_file" _R1.fastq.gz)
    
    # Define the corresponding R2 file
    r2_file="${base}_R2.fastq.gz"
    
    # Check if the corresponding R2 file exists
    if [ -f "$r2_file" ]; then
        echo "Processing $base with Kraken2..."
        
        # Run Kraken2 and output to the kraken_on_reads directory
        kraken2 --threads 8 \
            --db /home/jovyan/shared-public/db/kraken2/pluspf_8gb/latest \
            --output kraken_on_reads/${base}_reads_hits.txt \
            --report kraken_on_reads/${base}_reads_report.txt \
            --use-names \
            "$r1_file" "$r2_file"
        
        echo "Kraken2 completed for $base."
        
        # Run Krona to generate a taxonomy report and output to the krona_on_reads directory
        echo "Generating Krona report for $base..."
        ktImportTaxonomy -t 5 -m 3 kraken_on_reads/${base}_reads_report.txt -o krona_on_reads/${base}_krona_report.html
        
        echo "Krona report generated for $base."
    else
        echo "Warning: No matching R2 file for $r1_file. Skipping."
    fi
done

# Run MultiQC over the kraken_on_reads directory and save the result in the multiqc_kraken_on_reads directory
echo "Running MultiQC on Kraken2 reports..."
multiqc kraken_on_reads -o multiqc_kraken_on_reads
echo "MultiQC report generated in multiqc_kraken_on_reads."
```

What is the script doing? Spend five minutes trying to work out for yourself, then ask ChatGPT for a detailed explanation. Why was the pluspf_8gb database  chosen for kraken2 and when might other databases be appropriate?

---

Look at the individual **Krona plots** you have created and look at the **MultiQC Kraken** report. Remember to press the `Trust HTML` button each time.

 - You might find it more informative in the multiqc report to look at percentages than read counts.
 - What do you see that is notable in each sample?
 - Why aren't the results as informative as those on the NCBI website?

---

## Time for lunch

