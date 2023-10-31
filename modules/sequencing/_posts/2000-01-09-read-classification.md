# Read classification of our sequenced data

## Installing Kraken2 and Krona
We will need the following software:

* `kraken2` - a taxonomic profiler for metagenomics data
* `krona` - an interactive visualiser for the output of Kraken2
* `krakentools` - some useful scripts for manipulating Kraken2 output
* `taxpasta` - a useful tool for converting and merging Kraken2 outputs into other formats like Excel
* `busco`


```bash
conda install -c conda-forge -c bioconda busco=5.5.0
conda install -y kraken2 krona blast krakentools 
```

Krona reminds us to run `ktUpdateTaxonomy.sh` before it can be used.

```bash
ktUpdateTaxonomy.sh
```
You can test kraken2 was installed correctly by running it with no command-line options.

```bash
kraken2
```

## Kraken2 databases 
Pre-computed Kraken2 databases are available on the `/shared/public` file system within CLIMB-BIG-DATA. These databases are downloaded from Ben Langmad's publicly available Kraken2 indexes page. These datasets are updated monthly and we will keep the latest versions available.

The `/shared/public` area is designed to store frequently used, important databases for the microbial genomics community. We are just getting started building this resource so please contact us with suggestions for other databases you would like to see here.

We can take a look at the databases that are available, and their sizes:

```
du -h -d1 /shared/public/db/kraken2

7.5G    /shared/public/db/kraken2/k2_pluspf_08gb
9.1G    /shared/public/db/kraken2/k2_minusb
556M    /shared/public/db/kraken2/k2_viral
69G /shared/public/db/kraken2/k2_pluspf
7.5G    /shared/public/db/kraken2/k2_standard_08gb
15G /shared/public/db/kraken2/k2_pluspf_16gb
65G /shared/public/db/kraken2/k2_standard
15G /shared/public/db/kraken2/k2_standard_16gb
264G    /shared/public/db/kraken2/downloads
7.6G    /shared/public/db/kraken2/k2_pluspfp_08gb
15G /shared/public/db/kraken2/k2_pluspfp_16gb
145G    /shared/public/db/kraken2/k2_pluspfp
619G    /shared/public/db/kraken2
```

We can run Kraken2 directly within this JupyterHub notebook which is running in a container. A standard container has 8 CPu cores and 64Gb of memory. Kraken2 doesn't run well unless the database fits into memory, so we can use one of the smaller databases for now such a k2_standard_16gb which contains archaea, bacteria, viral, plasmid, human and UniVec_Core sequences from RefSeq, but subsampled down to a 16Gb database. This will be fast, but we trade off specificity and sensitivity against bigger databases.

```
kraken2 --threads 8 \
   --db /shared/public/db/kraken2/k2_standard_16gb \
   --output canalseq.hits.txt \
   --report canalseq.report.txt \
   canalseq.fasta

Loading database information... done.
37407 sequences (91.32 Mbp) processed in 4.876s (460.3 Kseq/m, 1123.76 Mbp/m).
  12486 sequences classified (33.38%)
  24921 sequences unclassified (66.62%)
About a third of sequences were classified and two-thirds were not.
```

The canalseq.report.txt gives a human-readable output from Kraken2.

```
cat canalseq.report.txt
```

It's easier to look at Kraken2 results visually using a Krona plot:

```
ktImportTaxonomy -t 5 -m 3 canalseq.report.txt -o KronaReport.html

   [ WARNING ]  Score column already in use; not reading scores.
Loading taxonomy...
Importing canalseq.report.txt...
Writing KronaReport.html...
```

We can look at the Krona report directly within the browser by using the file navigator to the left - open up the KronaReport.html within the shared-team directory where we are working. Click around the Krona report to see what is in there.

With the `extract_kraken_reads.py` script in krakentools we can quite easily extract a set of reads that we are interested in for further exploration: perhaps to use a more specific method like BLAST against a large protein database, or to extract for de novo assembly.

```
extract_kraken_reads.py -k canalseq.hits.txt -s canalseq.fasta -r canalseq.report.txt -t 171 -o leptospira.fasta --include-children

extract_kraken_reads.py -k canalseq.hits.txt -s canalseq.fasta -r canalseq.report.txt -t 173 -o linterrogens.fasta --include-children
```

```
PROGRAM START TIME: 04-16-2023 18:40:00
>> STEP 0: PARSING REPORT FILE canalseq.report.txt
    1 taxonomy IDs to parse
>> STEP 1: PARSING KRAKEN FILE FOR READIDS canalseq.hits.txt
    0.04 million reads processed
    2 read IDs saved
>> STEP 2: READING SEQUENCE FILES AND WRITING READS
    2 read IDs found (0.02 mill reads processed)
    2 reads printed to file
    Generated file: linterrogens.fasta
PROGRAM END TIME: 04-16-2023 18:40:00
```

If you wished you could go and take these reads and BLAST them over at NCBI-BLAST. There is also the `nr` BLAST database available on CLIMB-BIG-DATA if you wanted to run blastx on them. The BLAST databases are found in `/shared/team/db/blast`.