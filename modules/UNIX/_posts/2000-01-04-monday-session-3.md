---
title: Monday - Session 3
---

# Useful shell commands

> **Things covered here:**
>
> Here we’re going to touch upon 6 essential commands that are absolutely worth having in our toolkit!


Let’s change back into our starting unix_intro directory:

```bash
cd ~/unix_intro
```

We’ll mostly be working with a file here called “gene_annotations.tsv”, which is a tab-delimited table holding gene annotation information. Let’s change into our working directory for this page and explore it a little on the command line.

```bash
cd gene_annotations/
ls
```

## head and tail

The `head` command is used to show a file from the beginning:

```bash
head gene_annotations.tsv
```

By default `head` prints out the first 10 lines, but we can change this behaviour using the `-n` flag by specifying the number or lines to print. 

```bash
head -n 1 gene_annotations.tsv
head -n 5 gene_annotations.tsv
```

The `tail` command is used to show a file from the end and like with `head` you can also specify the number of lines to print with the `-n` flag.

```bash
tail gene_annotations.tsv
tail -n 1 gene_annotations.tsv
tail -n 5 gene_annotations.tsv
```

`head` and `tail` are powerful common commands and they can even be used to skip lines. For example specifying `-n +2` with `tail` will print the entire file starting from the second row - this might come in handy if we ever need to skip a header row.

```bash
# Skip the first line (header)
tail -n +2 gene_annotations.tsv
```

As we can see the bed file contains 4 columns (or fields): 
- `chromosome` specifies the chromosome where the gene is located, 
- `start` and `end` define the range of bases where the gene is located within that chromosome,
- `gene_ID` holds some descriptive information about the genes, like gene IDs and functional annotations.

## less

the `less` command allows us to browse contents of text files interactively, one page at a time. 

```bash
less gene_annotations.tsv
```

Now that we have opened the file in `less`, we can scroll up and down using the &uarr; and &darr; arrow keys respectively, or we can "turn" a page by pressing the `Space bar`, `Enter` or the `f` key. We can "turn" the page back by pressing the `b` key. If are ready to quit `less`, we can do so using the `q` button. 

There are other powerful things `less` can help us with which is beyond the scope of this lesson, but feel free to have a read about these using the `less --help` option.

Now that we know something about the file we’re working with, let’s get to some exciting commands!

## cut

`cut` is a command that’s great for manipulating columns. The required arguments for `cut` are which columns we want, and from which file. Here’s how we can use `cut` to pull out just the chromosome column (column 1):

```bash
cut -f 1 gene_annotations.tsv
```

That is printing out all of the lines to the screen though, let’s pipe it into head for now to keep things manageable while we’re working on it **(remember we can bring up a previous command by pressing up)**:

```bash
cut -f 1 gene_annotations.tsv | head
```

Here we are specifying which column we want with the `-f` parameter (for field). We can specify multiple individual columns if we separate them with a comma:

```bash
cut -f 1,3 gene_annotations.tsv | head
```

And a range of columns if we join them with a dash:

```bash
cut -f 1-3 gene_annotations.tsv | head
```

Just like in spreadsheet programs, `cut` thinks about where columns start and stop based on a delimiter, like a comma or a tab. The default setting is a tab, so we didn’t need to change it for that file. But if we try using it on a comma separated values file (a csv), things don’t work well:

```bash
cut -f 1-3 gene_annotations.csv | head
```

Unless we tell cut the delimiter is a comma, which we can do with the `-d` parameter:

```bash
cut -d "," -f 1-3 gene_annotations.csv | head
```

<blockquote>
<center><strong>QUICK PRACTICE!</strong></center>

From our tab-delimited file, <strong>"gene_annotations.tsv"!</strong>, try to make a new file that has just 2 columns: the chromosome and the gene_ID columns (remember the > redirector). <br>
Name the new file <strong>"chrom_and_IDs.tsv"</strong>.

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">
cut -f 1,4 gene_annotations.tsv | head
cut -f 1,4 gene_annotations.tsv > chrom_and_IDs.tsv
head chrom_and_IDs.tsv
</code></pre>
<p>And to make sure it holds all 199 lines and not just the first 10!</p>
<pre><code>
wc -l chrom_and_IDs.tsv
</code></pre>
</details>
</blockquote>


### grep

`grep` (**g**lobal **r**egular **e**x**p**ression) is a search tool. It looks through text files for strings (sequences of characters). In its default usage, `grep` will look for whatever string of characters you give it (1st positional argument), in whichever file you specify (2nd positional argument), and then print out the lines that contain what you searched for. Let’s try it:

```bash
grep AT1G08337 gene_annotations.tsv
```
The output is one gene on chromosome 1, encoding for a miRNA:

```bash
chr1	22372461	22372578	ID=AT1G08337;Name=MIR5022;locus_type=mirna
```

If there are multiple lines that match, grep will print them all:

```bash
grep 8337 gene_annotations.tsv


chr1	18333622	18337520	ID=AT1G49540;Name=AT1G49540;full_name=elongator
chr1	22372461	22372578	ID=AT1G08337;Name=MIR5022;locus_type=mirna```
```

If what we are looking for is not in the file, we will just get our prompt back with nothing printed out:

```bash
grep mygene gene_annotations.tsv
```

For the moment, let’s pretend we’re interested in a gene predicted to encode for DEMETER, (a DNA glycosylase protein, responsible for DNA hypomethylation) which has the gene ID AT5G04560. With `grep` we can check if it is in our annotations file:

```bash
grep AT5G04560 gene_annotations.tsv

chr5	1309098	1318520	ID=AT5G04560;Name=AT5G04560;full_name=DEMETER;computational_description=HhH-GPD
```

> **Note:** `grep` is case sensitive by default, but we can make it case insensitive with the `-i` flag. Try searching for DEMETER with and without this flag and see what you get:
>
> ```
> grep demeter gene_annotations.tsv
> grep -i demeter gene_annotations.tsv
> ```

<br>

<blockquote>
<center><strong>QUICK PRACTICE!</strong></center>

Using a combination of <code>grep</code> and <code>cut</code>, how could we print out the gene IDs of only the genes located on chromosome 2?

<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">
grep "chr2" gene_annotations.tsv | cut -f 4
</code></pre>
<p>Being able to stick together these individual pieces like this becomes a really valuable toolset to have at our disposal!
</p>
</details>
</blockquote>

We’re just scratching the surface of what `grep` can do, but one flag in addition to `-i` worth mentioning is the `-c` flag. This tells grep to just report how many lines matched, instead of printing them to the screen:

```bash
grep -c chr2 gene_annotations.tsv
```

### Optional: awk

`awk` is even more expansive than any of the others we’ve seen, but like the others, just being familiar with its basic command-line usage can be powerful. `awk` is useful for doing things like filtering based on columns and doing calculations.

The syntax of `awk` can also take a little getting used to. In awk, we specify columns with a `$` followed by the column number. For example, `$1` is column 1 (chromosome), `$2` is start position, etc.

Let's start with a simple example recreating `cut -f 1`: printing just the first column (chromosome):

```bash
awk '{print $1}' gene_annotations.tsv | head
```

Just like with `cut`, we can also print multiple columns:

```bash
awk '{print $1, $4}' gene_annotations.tsv | head
```
One of the most powerful uses of `awk` is filtering based on conditions. Let's say we only want genes from chromosome 3 (recreating `grep chr3 gene_annotations.tsv`):

```bash
awk '$1 == "chr3"' gene_annotations.tsv | head
```

We can also filter based on numeric comparisons. Let's find genes that start after position 1000000:

```bash
awk '$2 > 1000000' gene_annotations.tsv | head
```

`awk` can also perform calculations on the fly. Let's print the first 3 columns and create a new column that calculates the length of each gene (end position minus start position) and save it to a new file:

```bash
awk 'NR > 1 {print $1, $2, $3,$4, $3-$2}' gene_annotations.tsv > gene_annotations_and_lengths.tsv
```

The `NR > 1` part skips the first line (the header), so we don't try to do maths on text!

We can combine filtering and calculations. Let's find genes longer than 5000 base pairs:

```bash
awk '$5 > 5000' gene_annotations_and_lengths.tsv | head
```

Again, `awk` can seem pretty tricky, especially at first, but forunately we don’t need to remember how to do these things, just that they can be done. And then we can look it up when we need it 🙂


<blockquote>
<center><strong>QUICK PRACTICE!</strong></center>
How many protein coding genes in our list are larger than 2000bp?
<br>
<strong>Hint: Try to use <code>grep</code> and <code>awk</code> piped together.</strong> <br>
<details>
<summary>Solution</summary>
<br>
<pre><code class="language-bash">
grep protein_coding gene_annotations_and_lengths.tsv | awk '$5 > 2000'  | wc -l
</code></pre>
</details>
</blockquote>


## Summary

As mentioned, this page is just a first introduction to some great commands that are worth having in our toolkit. Each of them has much more functionality that we can dig into further as needed!

#### **Commands introduced:**

| Command &nbsp;&nbsp;&nbsp;&nbsp;   | Function (base-usage)                                                  |
|:-----------------------------------| -----------------------------------------------------------------------|
| `head` &nbsp;&nbsp;&nbsp;&nbsp;    | prints the first n lines (10 by default, specify with `-n`)            |
| `tail` &nbsp;&nbsp;&nbsp;&nbsp;    | prints the last n lines (10 by default, specify with `-n`)             |
| `less`&nbsp;&nbsp;&nbsp;&nbsp;     | browse file contents interactively (scroll with arrows, exit with `q`) |
| `cut` &nbsp;&nbsp;&nbsp;&nbsp;     | extracts columns from delimited files                                  |
| `grep` &nbsp;&nbsp;&nbsp;&nbsp;    | searches for text patterns and returns matching lines                  |
| `awk` &nbsp;&nbsp;&nbsp;&nbsp;     | filters rows and columns, performs calculations                        |

#### Acknowledgements 

This training course was adapted from the [Happy Belly Bioinformatics Unix Course](https://astrobiomike.github.io/unix/).
 
