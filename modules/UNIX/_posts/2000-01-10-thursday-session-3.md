---
title: Thursday - Session 3
---

# BLAST on the command line

> **Things covered here:**
>
> - What is BLAST and why run it locally?
> - Downloading data and building a BLAST database
> - Running a protein BLAST search
> - Understanding BLAST output formats
> - Exploring tabular results with shell tools

## Getting started

Make a directory to work in and move into it:

```bash
mkdir -p ~/unix_intro/blast
cd ~/unix_intro/blast
```

### Installing BLAST

If you are working locally, create a new environment and install BLAST:

```bash
mamba create -n blast-env blast=2.17.0
mamba activate blast-env
```

If you are on CLIMB, activate the shared environment:

```bash
mamba activate /shared/team/conda/judittalas.mmb-dtp/blast-env
```

Verify it worked:

```bash
blastp -version
```

## What is BLAST?

BLAST is the **B**asic **L**ocal **A**lignment **S**earch **T**ool. It finds regions of similarity between sequences by finding small matches and extending them. For more information on how BLAST works, check out the summary on [Wikipedia](https://en.wikipedia.org/wiki/BLAST_(biotechnology)) or the NCBI's [BLAST resources](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs).

BLAST can be useful for identifying the source of a sequence, or finding a similar sequence in another organism. In this tutorial, we will use BLAST to find zebrafish proteins that are similar to a small set of mouse proteins.

### Why use the command line?

BLAST has a nice graphical interface on the [NCBI website](https://blast.ncbi.nlm.nih.gov/Blast.cgi). However, running BLAST on the command line has several advantages:

- You can run many queries at once rather than pasting them in one at a time
- Your analysis is reproducible and can be documented in a script
- Results can be saved in a machine-readable format for downstream analysis
- You can create and search your own custom databases
- Searches can be automated as part of a larger workflow

## Downloading the data

We'll use `curl` to download mouse and zebrafish RefSeq protein sequences. These files originally came from the NCBI FTP site.

```bash
curl -o mouse.1.protein.faa.gz -L https://osf.io/v6j9x/download
curl -o zebrafish.1.protein.faa.gz -L https://osf.io/68mgf/download
```

> **Reminder:** `curl` downloads a file from a URL. The `-o` flag specifies the output filename, and `-L` tells curl to follow redirects.

Check that the files downloaded:

```bash
ls -lh
```

You should see two `.faa.gz` files. These are FASTA protein files (`.faa`) compressed with gzip (`.gz`). Uncompress them:

```bash
gunzip *.faa.gz
```

Let's look at the first few sequences:

```bash
head mouse.1.protein.faa
```

These are protein sequences in FASTA format. Each record starts with a header line beginning with `>`, followed by the amino acid sequence. The header contains an accession number and a description of the protein.

## Preparing query sequences

Rather than searching with the entire mouse proteome, let's start small. We'll extract the first two protein sequences and save them to a new file:

```bash
head -n 11 mouse.1.protein.faa > mm-first.faa
```

Check what you've got:

```bash
cat mm-first.faa
```

You should see two FASTA records (two `>` header lines, each followed by sequence).

## Building a BLAST database

Before we can search, we need to tell BLAST to index the zebrafish sequences as a database. This is done with `makeblastdb`:

```bash
makeblastdb -in zebrafish.1.protein.faa -dbtype prot
```

The `-dbtype prot` flag tells BLAST these are protein sequences (use `nucl` for nucleotide databases). This creates several index files alongside the original FASTA file. You can see them with:

```bash
ls zebrafish.1.protein.faa*
```

## Running BLAST

Now we can search our mouse proteins against the zebrafish database using `blastp` (protein-vs-protein BLAST). We give it our query file (`-query`), the database to search against (`-db`), and where to save the results (`-out`):

```bash
blastp \
    -query mm-first.faa \
    -db zebrafish.1.protein.faa \
    -out mm-first.x.zebrafish.txt
```

Page through the results:

```bash
less mm-first.x.zebrafish.txt
```

(Spacebar to scroll down, `q` to quit.)

This is BLAST's default human-readable output. It shows detailed alignments with matching residues, gaps, and statistics for each hit. It's useful for examining individual alignments, but not practical for working with many results at once.

## Tabular output with `-outfmt 6`

Let's now search with a larger set of queries and switch to a machine-readable output format. We'll extract the first 96 mouse proteins:

```bash
head -n 498 mouse.1.protein.faa > mm-second.faa
```

And run BLAST with `-outfmt 6`, which produces a tab-separated table:

```bash
blastp \
    -query mm-second.faa \
    -db zebrafish.1.protein.faa \
    -out mm-second.x.zebrafish.tsv \
    -outfmt 6
```

This will take a bit longer than our first search.
Let's look at the first few lines of output:

```bash
head mm-second.x.zebrafish.tsv
```

### The 12 default columns

The default `-outfmt 6` output always has these 12 columns, in this order:

| Column | Name       | What it tells you |
|--------|------------|-------------------|
| 1      | `qseqid`   | **Query ID** - the identifier of your query sequence (the mouse protein) |
| 2      | `sseqid`   | **Subject ID** - the identifier of the database hit (the zebrafish protein) |
| 3      | `pident`   | **Percent identity** - percentage of residues in the aligned region that are identical |
| 4      | `length`   | **Alignment length** - how many positions the alignment covers (including gaps) |
| 5      | `mismatch` | **Mismatches** - positions where residues differ (not counting gaps) |
| 6      | `gapopen`  | **Gap openings** - number of gaps introduced in the alignment |
| 7      | `qstart`   | **Query start** - position in the query where the alignment begins |
| 8      | `qend`     | **Query end** - position in the query where the alignment ends |
| 9      | `sstart`   | **Subject start** - position in the subject where the alignment begins |
| 10     | `send`     | **Subject end** - position in the subject where the alignment ends |
| 11     | `evalue`   | **E-value** - the expect value (see below) |
| 12     | `bitscore` | **Bit score** - a normalised alignment quality score (see below) |

Please see [this link](https://www.metagenomics.wiki/tools/blast/blastn-output-format-6) for further reading on output formats. 
### Key columns for interpreting your results

While all 12 columns are useful, three matter most when deciding whether a hit is meaningful:

#### Percent identity (column 3)

This tells you how similar the sequences are in the aligned region. Be careful though: a short alignment at 95% identity might be less meaningful than a long alignment at 40% identity. Always consider percent identity together with alignment length (column 4).

#### E-value (column 11)

The E-value answers the question: **"If I searched a database of this size with a random sequence, how many hits this good would I expect by chance?"**

- `1e-150` = essentially zero chance this is a random match, so this is a very strong hit.
- `1e-5` (0.00001) = commonly used as a rough significance threshold.
- `0.05` = you'd expect a hit this good about 1 in 20 random searches.
- `10` = you'd expect 10 hits this good by chance.

**Lower E-values = more significant hits.** But note that E-values depend on database size. The same alignment will produce a different E-value if you search a larger or smaller database, so you cannot directly compare E-values across different BLAST searches.

#### Bit score (column 12)

The bit score is a normalised alignment score that **does not** depend on database size. If you need to compare hits from different BLAST searches, use bit scores rather than E-values.


## Exploring results with shell tools

Since `-outfmt 6` produces a plain TSV, we can use all the shell tools from earlier this week. This is one of the main advantages of running BLAST on the command line.

How many hits did we get?

```bash
wc -l mm-second.x.zebrafish.tsv
```

How many unique mouse queries produced hits?

```bash
cut -f1 mm-second.x.zebrafish.tsv | sort -u | wc -l
```

Which zebrafish proteins were hit?

```bash
cut -f2 mm-second.x.zebrafish.tsv | sort -u | head
```

How many unique zebrafish proteins were hit?

```bash
cut -f2 mm-second.x.zebrafish.tsv | sort -u | wc -l
```

Notice we can build up commands incrementally: first look at the IDs, then count them. This is a useful habit when working on the command line.

<blockquote>
<center><strong>TASKS</strong></center>

Using the shell tools from earlier this week, try to answer the following:<br>

<strong>1. Filter the results to show only strong hits with an E-value below 1e-50.</strong>
<br><em>Hint: the E-value is in column 11. <code>awk</code> can compare numbers directly.</em>

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">awk '$11 < 1e-50' mm-second.x.zebrafish.tsv | head</code></pre>
</details>

<strong>2. Sort the results by percent identity (column 3), highest first. </strong>
<br><em>Hint: look at the <code>-k</code>, <code>-n</code>, and <code>-r</code> flags for <code>sort</code>.</em>

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">sort -k3,3nr mm-second.x.zebrafish.tsv | head</code></pre>
</details>

<strong>3. Sort the results by bit score (column 12) instead. </strong>

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">sort -k12,12nr mm-second.x.zebrafish.tsv | head</code></pre>
</details>

<strong>4. Write a one-liner that shows each query sequence and how many hits it has, sorted from most to fewest. </strong>
<br><em>Hint: you'll need <code>cut</code>, <code>sort</code>, <code>uniq -c</code>, and <code>sort</code> again.</em>

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">cut -f1 mm-second.x.zebrafish.tsv | sort | uniq -c | sort -nr | head</code></pre>
</details>
</blockquote>

## Adding a header line

One annoyance with `-outfmt 6` is that there's no header. You can add one:

```bash
echo -e "qseqid\tsseqid\tpident\tlength\tmismatch\tgapopen\tqstart\tqend\tsstart\tsend\tevalue\tbitscore" > mm-second.with-header.tsv
cat mm-second.x.zebrafish.tsv >> mm-second.with-header.tsv
```

The first line writes just the header to a new file (`>`). The second line appends the data to the same file (`>>`). Now `head mm-second.with-header.tsv` will show column names alongside the data.

> **Tip:** `-outfmt 7` gives you the same tabular format as `-outfmt 6` but with comment lines (starting with `#`) that include column headers, the query ID, database name, and number of hits. Try running the search again with `-outfmt 7` and compare the output.

## Customising the output columns

You're not limited to the default 12 columns. You can specify exactly which fields you want:

```bash
blastp -query mm-first.faa -db zebrafish.1.protein.faa \
    -outfmt "6 qseqid sseqid evalue pident qcovs stitle" \
    -out mm-first.custom.tsv
```

This adds `qcovs` (percent of query covered) and `stitle` (full description of the hit), and drops the columns we didn't ask for. To see the full list of available fields, search the BLAST help text:

```bash
blastp -help | less
```

Then type `/alignment view options` and press Enter to jump to the format specifiers section. Press `q` to quit when you're done.

### Acknowledgements

This tutorial was adapted from the [Running Command-Line BLAST](https://github.com/ngs-docs/angus/blob/2019/running-command-line-blast.md) lesson from the ANGUS (Analyzing Next-Generation Sequencing) course materials. The original tutorial data files are hosted on the [Open Science Framework](https://osf.io/).