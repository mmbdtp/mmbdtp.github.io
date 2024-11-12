---
title: Tuesday afternoon - Initial analysis of MAGs
---


# Tuesday afternoon 
## Initial analysis of MAGs

---


## Objectives 
By the end of this session, you will be able to:

1. **Evaluate Metagenome-Assembled Genomes (MAGs)**:
   - Run and interpret BLAST searches on sequences from MAGs to identify potential organisms and their coherence in results.
   - Analyze marker gene sequences extracted from MAGs with BLASTP and interpret findings.

2. **Assess Quality of MAGs Using CheckM**:
   - Understand and interpret the outputs from CheckM, focusing on completeness, contamination, and the overall quality of the bins.
   - Learn how completeness and contamination metrics are combined into a quality score for evaluating the reliability of MAGs.

3. **Validate MAGs with Kraken**:
   - Use Kraken to further classify and validate MAGs by comparing the taxonomic profiles obtained.
   - Identify if the classified organisms align with expected sample content, noting the presence of pathogens or commensal organisms.

4. **Synthesize and Report Findings**:
   - Compare and cross-reference results from BLAST, CheckM, and Kraken to build a coherent picture of the bin's quality and taxonomic identity.
   - Conclude with a discussion on the reliability of the MAGs and potential next steps for further analysis.

---


## Playing with BLAST

Open each bin and copy and paste the first dozen or so sequences into a [BLAST window](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastn&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome)
Select "1" as the value in the "Max matches in query range" box to restrict results to one hit per input sequence.
Use the "Download All" option on the second line and select "text" to get a handy summary of results.

 - How do you interpret what you are seeing?
 - Do you get coherent results?

Now decompress the .marker_of_each_bin.tar.gz file, e.g.

```
tar -xzvf maxbin_out_patient02_day01.marker_of_each_bin.tar.gz

```

You will now get files called something like `maxbin_out_patient02_day01..001.marker.fasta`.
Open each file and do a BLASTP search with all the marker protein sequences in it. As before, select "1" as the value in the "Max matches in query range" box to restrict results to one hit per input sequence.
Use the "Download All" option on the second line and select "text" to get a handy summary of results.

 - How do you interpret what you are seeing?
 - Do you get coherent results?

---


## CheckM

**CheckM** is a bioinformatics tool used for assessing the quality of metagenome-assembled genomes (MAGs) or genomic bins by evaluating their completeness and contamination. It employs a lineage-specific workflow that leverages a database of single-copy marker genes to estimate these metrics. 

**Completeness** is determined by checking the presence of essential marker genes that are expected to be found in single copies within a complete genome. A higher proportion of these marker genes indicates a more complete genome. 

**Contamination**, on the other hand, is assessed by identifying instances where single-copy marker genes appear multiple times, suggesting the presence of mixed genomic material or fragments from different organisms. 

Low contamination (typically below 5%) and high completeness (often above 90% for high-quality MAGs) are desirable indicators of a well-assembled, reliable genome. CheckM's results help researchers determine the quality of their genomic bins and guide decisions on further analysis or refinement.

Completeness and contamination metrics provided by **CheckM** are often combined to assess the overall quality of a metagenome-assembled genome (MAG) using a single metric called the **quality score** or **completeness-contamination trade-off**. This combined metric helps researchers quickly judge the reliability of a MAG by balancing the desirable traits of high completeness and low contamination.

### Calculating a Quality Score:
The quality of a MAG can be evaluated using a formula that incorporates both completeness and contamination. A commonly used approach is:
\[
\text{Quality Score} = \text{Completeness} - ( \text{Contamination} \times \text{Penalty Factor} )
\]

- **Completeness**: The proportion of expected single-copy marker genes present in the MAG.
- **Contamination**: The proportion of duplicated single-copy marker genes, suggesting extraneous or mixed genomic content.
- **Penalty Factor**: A weighting factor (often set to 5) that emphasizes the negative impact of contamination.

### Interpretation:
- A higher **Quality Score** reflects a more reliable and high-quality MAG, with completeness contributing positively and contamination reducing the score.
- For example, a MAG with 95% completeness and 2% contamination would be scored as:
\[
\text{Quality Score} = 95 - (2 \times 5) = 85
\]

This metric allows researchers to quickly compare bins and decide which ones are worth focusing on for downstream analysis, as it takes into account both the presence of essential genes and the purity of the genomic bin. High-quality MAGs typically have a high completeness (e.g., ≥90%) and low contamination (e.g., ≤5%), leading to a strong quality score that indicates confidence in the assembly's reliability.

---


### Running CheckM
Run CheckM over all the bins in a MaxBin output directory:

```
checkm lineage_wf ./. checkm_output/ -t 16 -x fasta
```

The output will look something like this:

```
[2024-11-12 08:14:42] INFO: CheckM v1.2.3
[2024-11-12 08:14:43] INFO: checkm lineage_wf ./. checkm_output/ -t 16 -x fasta
[2024-11-12 08:14:43] INFO: CheckM data: /shared/team/conda/mpallen.mmb-dtp/seqanalysis/checkm_data
[2024-11-12 08:14:43] INFO: [CheckM - tree] Placing bins in reference genome tree.
[2024-11-12 08:14:43] WARNING: Expected all files to contain sequences in nucleotide space.
[2024-11-12 08:14:43] WARNING: File ././maxbin_bins.001.marker.fasta appears to contain amino acids sequences.
[2024-11-12 08:14:43] WARNING: Expected all files to contain sequences in nucleotide space.
[2024-11-12 08:14:43] WARNING: File ././maxbin_bins.002.marker.fasta appears to contain amino acids sequences.
[2024-11-12 08:14:43] WARNING: Expected all files to contain sequences in nucleotide space.
[2024-11-12 08:14:43] WARNING: File ././maxbin_bins.003.marker.fasta appears to contain amino acids sequences.
[2024-11-12 08:14:43] INFO: Identifying marker genes in 7 bins with 16 threads:
    Finished processing 7 of 7 (100.00%) bins.
[2024-11-12 08:14:51] INFO: Saving HMM info to file.
[2024-11-12 08:14:51] INFO: Calculating genome statistics for 7 bins with 16 threads:
    Finished processing 7 of 7 (100.00%) bins.
[2024-11-12 08:14:51] INFO: Extracting marker genes to align.
[2024-11-12 08:14:51] INFO: Parsing HMM hits to marker genes:
    Finished parsing hits for 7 of 7 (100.00%) bins.
[2024-11-12 08:14:51] INFO: Extracting 43 HMMs with 16 threads:
    Finished extracting 43 of 43 (100.00%) HMMs.
[2024-11-12 08:14:51] INFO: Aligning 43 marker genes with 16 threads:
    Finished aligning 43 of 43 (100.00%) marker genes.
[2024-11-12 08:14:52] INFO: Reading marker alignment files.
[2024-11-12 08:14:52] INFO: Concatenating alignments.
[2024-11-12 08:14:52] INFO: Placing 7 bins into the genome tree with pplacer (be patient).
[2024-11-12 08:18:33] INFO: { Current stage: 0:03:50.463 || Total: 0:03:50.463 }
[2024-11-12 08:18:33] INFO: [CheckM - lineage_set] Inferring lineage-specific marker sets.
[2024-11-12 08:18:33] INFO: Reading HMM info from file.
[2024-11-12 08:18:33] INFO: Parsing HMM hits to marker genes:
    Finished parsing hits for 7 of 7 (100.00%) bins.
[2024-11-12 08:18:33] INFO: Determining marker sets for each genome bin.
    Finished processing 10 of 10 (100.00%) bins (current: maxbin_bins.001).     
[2024-11-12 08:18:34] INFO: Marker set written to: checkm_output/lineage.ms
[2024-11-12 08:18:34] INFO: { Current stage: 0:00:01.467 || Total: 0:03:51.931 }
[2024-11-12 08:18:34] INFO: [CheckM - analyze] Identifying marker genes in bins.
[2024-11-12 08:18:34] WARNING: Expected all files to contain sequences in nucleotide space.
[2024-11-12 08:18:34] WARNING: File ././maxbin_bins.001.marker.fasta appears to contain amino acids sequences.
[2024-11-12 08:18:34] WARNING: Expected all files to contain sequences in nucleotide space.
[2024-11-12 08:18:34] WARNING: File ././maxbin_bins.002.marker.fasta appears to contain amino acids sequences.
[2024-11-12 08:18:35] WARNING: Expected all files to contain sequences in nucleotide space.
[2024-11-12 08:18:35] WARNING: File ././maxbin_bins.003.marker.fasta appears to contain amino acids sequences.
[2024-11-12 08:18:35] INFO: Identifying marker genes in 7 bins with 16 threads:
    Finished processing 7 of 7 (100.00%) bins.
[2024-11-12 08:20:39] INFO: Saving HMM info to file.
[2024-11-12 08:20:39] INFO: { Current stage: 0:02:04.894 || Total: 0:05:56.826 }
[2024-11-12 08:20:39] INFO: Parsing HMM hits to marker genes:
    Finished parsing hits for 7 of 7 (100.00%) bins.
[2024-11-12 08:20:40] INFO: Aligning marker genes with multiple hits in a single bin:
    Finished processing 7 of 7 (100.00%) bins.
[2024-11-12 08:20:47] INFO: { Current stage: 0:00:07.710 || Total: 0:06:04.536 }
[2024-11-12 08:20:47] INFO: Calculating genome statistics for 7 bins with 16 threads:
    Finished processing 7 of 7 (100.00%) bins.
[2024-11-12 08:20:47] INFO: { Current stage: 0:00:00.300 || Total: 0:06:04.836 }
[2024-11-12 08:20:47] INFO: [CheckM - qa] Tabulating genome statistics.
[2024-11-12 08:20:47] INFO: Calculating AAI between multi-copy marker genes.
[2024-11-12 08:20:48] INFO: Reading HMM info from file.
[2024-11-12 08:20:48] INFO: Parsing HMM hits to marker genes:
    Finished parsing hits for 7 of 7 (100.00%) bins.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Bin Id                          Marker lineage          # genomes   # markers   # marker sets    0     1     2    3    4   5+   Completeness   Contamination   Strain heterogeneity  
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  maxbin_bins.002              k__Bacteria (UID203)          5449        104            58         21    56    25   1    1   0       76.61           16.60               0.00          
  maxbin_bins.003            g__Klebsiella (UID5140)          31         1312          336        601   574   119   15   3   0       54.41            9.71               2.31          
  maxbin_bins.001          f__Lachnospiraceae (UID1256)       33         333           171        192   129    12   0    0   0       41.31            2.38              16.67          
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
[2024-11-12 08:20:49] INFO: { Current stage: 0:00:01.745 || Total: 0:06:06.582 }

```

---


### Explanation:
The provided CheckM output log includes various stages of processing, warnings, and a summary table of results. Here's a breakdown of what CheckM is doing and the significance of each part of the output:

1. **Initialization**:
   - `[2024-11-12 08:14:42] INFO: CheckM v1.2.3`: Indicates the version of CheckM being used.
   - `[2024-11-12 08:14:43] INFO: checkm lineage_wf ./. checkm_output/ -t 16 -x fasta`: The `lineage_wf` command is being run to evaluate genome bins. The parameters specify the input directory (`./.`), the output directory (`checkm_output/`), the use of 16 CPU threads (`-t 16`), and that the input files have the `.fasta` extension (`-x fasta`).

2. **Warnings**:
   - `[2024-11-12 08:14:43] WARNING: Expected all files to contain sequences in nucleotide space`: Indicates that some input files do not contain nucleotide sequences as expected but may contain amino acid sequences instead.
   - **Specific warnings for files** (`maxbin_bins.001.marker.fasta`, etc.): Warns that specific marker files appear to contain amino acid sequences, which could mean that non-nucleotide data were mistakenly used as input.

3. **Processing Stages**:
   - **Placing bins in the reference genome tree**: `[2024-11-12 08:14:43] INFO: [CheckM - tree] Placing bins in reference genome tree.` CheckM is aligning the bins to a reference genome tree to identify taxonomic lineages.
   - **Identifying marker genes**: `[2024-11-12 08:14:51] INFO: Identifying marker genes in 7 bins`: CheckM uses Hidden Markov Models (HMMs) to find marker genes in the bins, which are crucial for determining completeness and contamination.
   - **Aligning marker genes**: `[2024-11-12 08:14:51] INFO: Aligning 43 marker genes with 16 threads`: Aligns marker genes to refine genome bin quality analysis.
   - **Calculating genome statistics**: `[2024-11-12 08:20:47] INFO: Calculating genome statistics for 7 bins`: Calculates completeness and contamination based on the presence and redundancy of single-copy marker genes.

4. **Summary Table**:
   - **`Bin Id`**: The name of each bin being analyzed.
   - **`Marker lineage`**: The predicted taxonomic lineage based on marker genes.
   - **`# genomes`, `# markers`, `# marker sets`**: Counts indicating how many genomes, total markers, and marker sets were considered for analysis.
   - **Completeness and Contamination**:
     - **`Completeness`**: The percentage of expected single-copy marker genes found in the bin. A higher value indicates a more complete genome.
     - **`Contamination`**: The percentage of redundant single-copy marker genes, suggesting contamination. A lower value is better.
   - **`Strain heterogeneity`**: Indicates variation in marker genes that may suggest strain-level diversity within the bin. Values close to `0.00` are ideal, as they indicate minimal heterogeneity.

### Comments on the Output:
- **Warnings**: The warnings about amino acid sequences in the `marker.fasta` files suggest there may be an issue with the input files. Ensure that the inputs are nucleotide sequences, as CheckM is designed to work with DNA, not protein sequences.
- **Completeness and Contamination**: The table shows that `maxbin_bins.002` has reasonable completeness (76.61%) but high contamination (16.60%), suggesting it may represent a mixed bin or contain sequences from different organisms. `maxbin_bins.003` has moderate completeness (54.41%) and contamination (9.71%). The bins labeled as `.marker` have `0.00%` completeness and contamination, indicating that they do not contain enough data for meaningful analysis or might be erroneous.
- **Genome Quality**: The bin `maxbin_bins.001_1` shows low completeness (41.31%) but low contamination (2.38%), indicating it's a partial bin with less contamination but limited coverage.

---

### Common CheckM Output Files
**CheckM** produces several output files during the analysis of genome bins to assess their completeness and contamination. These files provide detailed insights into the quality and characteristics of the bins. Here's an overview of the typical CheckM output files and what each contains

1. **`lineage.ms`**:
   - **Description**: A file summarizing the lineage assignments of the genome bins.
   - **Contents**: Lists each bin and its assigned lineage based on the reference genome tree. This is crucial for understanding the expected marker genes for calculating completeness and contamination.
   - **Use**: Provides the basis for assessing which single-copy marker genes are expected for each bin, enabling further quality checks.

2. **`bin_stats_ext.tsv`**:
   - **Description**: A tab-delimited file containing detailed statistics for each bin.
   - **Contents**: Includes metrics such as genome size, number of contigs, GC content, number of predicted genes, completeness, contamination, and strain heterogeneity.
   - **Use**: Used for quickly comparing and assessing the quality of multiple bins. This is one of the main outputs to review for MAG quality.

3. **`storage/tree`**:
   - **Description**: A directory that stores the reconstructed phylogenetic tree generated during the analysis.
   - **Contents**: Contains the data files used for placing the bins in the reference genome tree.
   - **Use**: Provides a visualization of how bins are phylogenetically related to known lineages.

4. **`markers` Directory**:
   - **Description**: A directory containing data related to marker genes detected in each bin.
   - **Contents**: Contains files listing which marker genes were found and how many times they were present in each bin.
   - **Use**: Essential for understanding the presence and redundancy of marker genes, which are critical for completeness and contamination calculations.

5. **`checkm.log`**:
   - **Description**: A log file recording the progress and any issues encountered during the CheckM run.
   - **Contents**: Includes timestamps, processing details, warnings, and any errors that may have occurred.
   - **Use**: Valuable for troubleshooting and understanding the steps CheckM took during the analysis.

6. **`storage` Directory**:
   - **Description**: Contains intermediate data files generated by CheckM, which are used during the analysis.
   - **Contents**: Includes data on the marker gene alignments, placement in the genome tree, and results of the HMM searches.
   - **Use**: Typically not reviewed directly but necessary for re-running or extending the analysis.

7. **Summary Reports (`*.txt` or custom report files)**:
   - **Description**: Depending on the `checkm qa` command or additional commands run, you might have custom-generated summary reports.
   - **Contents**: Includes a formatted summary of each bin’s completeness, contamination, and overall quality metrics.
   - **Use**: These reports are often shared with collaborators or included in analysis reports to summarize the quality of genome bins.

---

### CheckM conclusions

So what do conclude from your CheckM results? How do these results measure up against what you found with BLAST?

---

## Kraken again

So, have we got credible MAGs? One additional line of attack is to run Kraken on all our bins, with a command something like this:

```
kraken2 --threads 32 --db /home/jovyan/shared-public/db/kraken2/pluspf_8gb/latest --output bins.001_kraken_hits.txt     --report bins.001_kraken_report.txt   --use-names  maxbin_bins.001.fasta
```

What do you conclude now?
 - Are the organisms you are seeing in your bins the organisms you would expect to see in this context?
 - Are you seeing potential human pathogens? Commensals?
 - Do a brief PubMed search if you are unfamiliar with the organisms you are seeing.


How does what we are seeing in MAGs measure up with what we know from profiling the metagenomes?

Take a quick look again at the NCBI's Krona plot on your sample.

<div style="font-size: 12px; font-family: Arial, sans-serif; margin: 20px; border: 1px solid #ddd; border-radius: 8px; overflow: hidden; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);">
  <table style="width: 100%; border-collapse: collapse;">
    <thead style="background-color: #f4f4f4;">
      <tr>
        <th style="padding: 10px; text-align: left; border-bottom: 2px solid #ccc;">Patient</th>
        <th style="padding: 10px; text-align: left; border-bottom: 2px solid #ccc;">ICU Day</th>
        <th style="padding: 10px; text-align: left; border-bottom: 2px solid #ccc;">Run Link</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom: 1px solid #ddd;">
        <td style="padding: 8px;">Patient 02</td>
        <td style="padding: 8px;">1</td>
        <td style="padding: 8px;"><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926116&display=analysis" target="_blank" style="color: #0066cc; text-decoration: none;">SRR8926116</a></td>
      </tr>
      <tr style="border-bottom: 1px solid #ddd;">
        <td style="padding: 8px;">Patient 02</td>
        <td style="padding: 8px;">10</td>
        <td style="padding: 8px;"><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926119&display=analysis" target="_blank" style="color: #0066cc; text-decoration: none;">SRR8926119</a></td>
      </tr>
      <tr style="border-bottom: 1px solid #ddd;">
        <td style="padding: 8px;">Patient 04</td>
        <td style="padding: 8px;">10</td>
        <td style="padding: 8px;"><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926181&display=analysis" target="_blank" style="color: #0066cc; text-decoration: none;">SRR8926181</a></td>
      </tr>
      <tr style="border-bottom: 1px solid #ddd;">
        <td style="padding: 8px;">Patient 04</td>
        <td style="padding: 8px;">14</td>
        <td style="padding: 8px;"><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926187&display=analysis" target="_blank" style="color: #0066cc; text-decoration: none;">SRR8926187</a></td>
      </tr>
      <tr style="border-bottom: 1px solid #ddd;">
        <td style="padding: 8px;">Patient 29</td>
        <td style="padding: 8px;">15</td>
        <td style="padding: 8px;"><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926127&display=analysis" target="_blank" style="color: #0066cc; text-decoration: none;">SRR8926127</a></td>
      </tr>
      <tr style="border-bottom: 1px solid #ddd;">
        <td style="padding: 8px;">Patient 35</td>
        <td style="padding: 8px;">3</td>
        <td style="padding: 8px;"><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926193&display=analysis" target="_blank" style="color: #0066cc; text-decoration: none;">SRR8926193</a></td>
      </tr>
      <tr>
        <td style="padding: 8px;">Patient 36</td>
        <td style="padding: 8px;">11</td>
        <td style="padding: 8px;"><a href="https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR8926233&display=analysis" target="_blank" style="color: #0066cc; text-decoration: none;">SRR8926233</a></td>
      </tr>
    </tbody>
  </table>
</div>


---

## End of the session 
That's all for today.
Tomorrow, we will analyse some of the MAGs in terms of phylogeny and function.

---

