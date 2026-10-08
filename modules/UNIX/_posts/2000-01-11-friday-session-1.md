---
title: Friday - Session 1
---

# Challenges

These tasks are designed to bring together everything you've learned this week. Work through them at your own pace and ask for help if you get stuck.

## Task 1: Bash Recap

<blockquote>
<center><strong>TASKS</strong></center>

<strong>1. Compress gene_annotations.tsv, then view its first 5 lines without decompressing it. </strong>

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">gzip gene_annotations.tsv
zcat gene_annotations.tsv.gz | head -n 5</code></pre>
</details>


<strong>2. Save the gene IDs of all protein-coding genes on chromosome 1 to chr1_coding.txt. How many are there? </strong>

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">grep "protein_coding" gene_annotations.tsv | awk -F'\t' '$1=="chr1" {print $4}' > chr1_coding.txt</code></pre>
There are 20 genes
</details>

<strong>3. Create a script that accepts a folder path and renames all the files to &lt;filename&gt;_old.&lt;ext&gt;. Make that script executable from any location.</strong>

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">folder="$1"
for path in "$folder"/*; do
    dir=$(dirname "$path")
    file=$(basename "$path")
 
    if [[ "$file" == *.* ]]; then
        name="${file%.*}"    # everything before the last dot
        ext="${file##*.}"    # everything after the last dot
        new="${name}_old.${ext}"
    else
        new="${file}_old"    # file has no extension
    fi
    mv "$path" "$dir/$new"
    echo "$file -> $new"
done
</code></pre>
</details>

</blockquote>

## Task 2: Mystery microbe

Make sure your BLAST environment is active before you start:

```bash
mamba activate /shared/team/conda/judittalas.mmb-dtp/blast-env
```

You have been given a 16S rRNA sequence (`mystery.fasta`) from an unknown bacterium, and a database of 16S sequences from different bacterial species. Your job is to figure out which species it most likely belongs to.

### Setup

```bash
cd ~/unix_intro/friday_blast
ls
```

<blockquote>
<center><strong>TASKS</strong></center>

<strong>1. Have a look at the database file. How many sequences are in it, and which species?</strong>

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">grep "^>" 16s_db.fasta</code></pre>
You should see six sequences from six different species.
</details>

<strong>2. Build a BLAST database from <code>16s_db.fasta</code>.</strong>
<br><em>Hint: these are nucleotide sequences, not proteins.</em>

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">makeblastdb -in 16s_db.fasta -dbtype nucl</code></pre>
</details>

<strong>3. BLAST the mystery sequence against the database and save the results as a TSV file.</strong>
<br><em>Hint: for nucleotide-vs-nucleotide searches, use <code>blastn</code> instead of <code>blastp</code>.</em>
<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">
blastn -query mystery.fasta \
    -db 16s_db.fasta \
    -out mystery_results.tsv \
    -outfmt 6</code></pre>
</details>

<strong>4. Look at the results. Which species is the best match? How can you tell?</strong>

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">cat mystery_results.tsv</code></pre>
With only six sequences in the database, the full results are small enough to read directly. The top hit with the highest percent identity is the most likely source species. You should see E. coli at ~99% identity, which is very close but not an exact match. This suggests the mystery sequence is from E. coli, but a different strain to the one in the database.
</details>

<strong>5. You should notice that most species appear once in the results, but one appears twice. Which one, and why might that be?</strong>

<details>
<summary>Solution</summary>
<br>
Campylobacter appears on two lines. The other species all align across ~1400 bp, but Campylobacter's best alignment is only ~936 bp, with a second shorter alignment of ~164 bp. The 16S sequence is so divergent between E. coli and Campylobacter that BLAST cannot align the full length in one stretch and splits it into two partial alignments. This is another signal of evolutionary distance on top of the lower percent identity.
</details>

<strong>6. Looking at the percent identities across all six species, can you group them by relatedness? Which species are more closely related to each other, and which are more distant?</strong>

<details>
<summary>Solution</summary>
<br>
You should see three tiers:
<ul>
<li>E. coli: ~99% identity (closest match, same species)</li>
<li>Salmonella, Enterobacter, and Klebsiella: ~96-97% cluster. These are all members of the family Enterobacteriaceae.</li>
<li>Pseudomonas: ~85%. It is a Gammaproteobacterium like the Enterobacteriaceae, but in a different family.</li>
<li>Campylobacter: ~80%. It belongs to a different class of bacteria entirely (Epsilonproteobacteria), which is also reflected in the shorter alignment length.</li>
</ul>
</details>
</blockquote>

## Task 3: The clean up

Imagine 12 months have gone by. You've worked on 7 different analysis since then and you're disk space is full. How would you go about tidying this project up?

1. Could you clearly identify which files are copies from public repositories or local archives? Do you need to keep them here?
2. Which files are intermediate files that could be easily regenerated? Do you need to keep them here?
3. Which files are critical outputs (scripts, plots, summary tables)? How might you minimise their current storage.

How might we design a folder structure to clearly delineate this in future analyses? 