##FASTA files
 
Here we introduce the FASTA format to store DNA and protein sequences.

The FASTA format is a simple text file format, and it’s as widely used as it is old (1985). Each sequence has a name, extra information (comments) and the sequence itself, and one or more sequences can be stored in the same file, one after the other. The sequence itself can span multiple lines.

Here an example:

    
        
```
>P01013 GENE X PROTEIN (OVALBUMIN-RELATED)
QIKDLLVSSSTDLDTTLVLVNAIYFKGMWKTAFNAEDTREMPFHVTKQESKPVQMMCMNNSFNVATLPAE
KMKILELPFASGDLSMLVLLPDEVSDLERIEKTINFEKLTEWTNPNTMEKRRVKVYLPQMKIEEKYNLTS
VLMALGMTDLFIPSANLTGISSAESLKISQAVHGAFMELSEDGIEMAGSTGVIEDIKHSPESEQFRADHP
FLFLIKHNPTNTIVYFGRYWSP

```  

*   The header line must start with a “>” character.
*   The sequence identifier, or sequence name, it’s P01013 (the first word before a white space)
*   The sequence comment, in this example, is “GENE X PROTEIN (OVALBUMIN-RELATED)”
*   The sequence itself is a protein sequence, commonly stored in uppercase (but for DNA it’s commont to find lowercase sequences too).

Download a FASTA file
-----------------------------

Using `wget` (alternatively, use `curl`), download this file.

    
```
wget "https://raw.githubusercontent.com/telatin/learn_bash/master/phage/vir_protein.faa"
```
or

```
curl -o vir_protein.faa "https://raw.githubusercontent.com/telatin/learn_bash/master/phage/vir_protein.faa"
```
    
    
Count the number of sequences
-----------------------------

For a FASTA file, the number of sequences is the number of lines starting with `>`. To anchor a pattern to the beginning of the line we have to prepend the “^” character. As _greater than_ characters are not allowed elsewhere, we can omit it, but it’s good to be more stringent.

    
```
grep -c "^>" vir_protein.faa
```
        
    

Alternatively, we can use `grep` to select the header lines, and then `wc` to count the number of lines (this is just a way to refresh how pipes work):

    
```
grep "^>" vir_protein.faa | wc -l
```
    

How many bases/aminoacids in total?
-----------------------------------

This task is slightly more complicated. To understand why, let’s create a dummy FASTA file:

    
```
echo ">seq1" > dummy.fa
echo "ATGC" >> dummy.fa
echo "GTCA" >> dummy.fa
```
        
It’s always a good idea to create a simple test case where the answer is known (1 sequence, 8 bases long).

Now, we can use `grep` to select the lines that do not start with `>`, and then `wc` to count the number of characters:

    
```
grep -v "^>" dummy.fa | wc -c
```
     

The `-v` option inverts the match, so we select the lines that do not start with `>`. The `wc -c` counts the number of characters, including the _newline_ characters. The total we receive is 10! So to get the answer we need to count all the characters, and then subtract the number of lines:

    
```
# Select with grep the lines NOT starting 
#with ">" and count their chars with wc
grep -v "^>" dummy.fa | wc -c   # this is 10

#Here we use grep to count the lines
# that do not start with ">"
grep -vc "^>" dummy.fa          # this is 2
    
# What's 10-2? Bash can tell us!
echo 10 - 2 | bc                # this gives 8, using a command line based calculator :)

```
    

The basic command line tools are fantastic but with this example we already understand the need for a dedicated tool for FASTA files (later).

Peeking in a FASTA file
-----------------------

To see the first 10 lines of a FASTA file, we can use `head`:

    
```
head -n 10 vir_protein.faa
```
        

Sometimes it’s useful to see the _names_ of the first sequences:

    
```
grep "^>" vir_protein.faa | head -n 10
    
```
        

Dealing with gzipped files
--------------------------

Often, FASTA files are gzipped, to save space. Let’s compress our file:

    
```
# Compress the file
gzip vir_protein.faa
    
# Check that a .gz file has been created in its place
ls -l vir_protein.faa*
```

To decompress a text file on the fly there are two options: `zcat` (not always available) or simply piping to `gzip -d` (to decompress):

```
cat vir_protein.faa.gz | gzip -d | head -n 10
```
or

```
zcat vir_protein.faa.gz | head -n 10
    
```
    

Using a dedicated tool: SeqFu
-----------------------------

Using [mamba (or conda)](/microbiome-bioinformatics/Install-Miniconda/) we can install the [`seqfu`](https://telatin.github.io/seqfu2/) tool:

    
```
# We request a version above 1.9 (at the time of writing, the last version is 1.16)
mamba install -c bioconda -c conda-forge "seqfu>1.9"
    
```
    

Now we can use the `seqfu` tool to count the number of sequences:

    
```
 seqfu count vir_protein.faa.gz
    
```
    

And we can collect more statistics with `seqfu stats`:

    
```
seqfu stats -nt vir_protein.faa.gz dummy.fa
    
```
    
Where:

*   `-n` will print a “nice” output, with a terminal table (default is TSV)
*   `-t` will add the thousands separator (default is off)

the output should be:

    
```    
    ┌-----------------┬------┬----------┬-------┬-----┬-----┬-------┐
    │ File            │ #Seq │ Total bp │ Avg   │ N50 │ Min │  Max  │
    ├-----------------┼------┼----------┼-------┼-----┼-----┼-------┤
    │ vir_protein.faa │ 73   │ 14,866   │ 203.6 │ 246 │ 28  │ 1,132 │
    │ dummy.fa        │ 1    │ 8        │ 8.00  │ 8   │ 8   │ 8     │
    └-----------------┴------┴----------┴-------┴-----┴-----┴-------┘
```
    

(We used also our dummy file to check that the tool works as expected!)

[**SeqFu**](https://telatin.github.io/seqfu2/) has a long list of tools, some are modeled after the Unix tools we have seen so far, but with a more powerful syntax and more options.

For example:

*   `seqfu head` will print the first _n_ sequences (also taking 1 every _y_)
*   `seqfu tail` will print the last _n_ sequences
*   `seqfu grep` can select sequences by name or by sequence, including degenerate IUPAC primers and partial matches, or matches in reverse complement
*   `seqfu cat` will concatenate sequences, with filters to remove short/long sequences or truncate/trim them

In addition there are sequence specific tools, such as:

*   `seqfu rc` to reverse complement one or more sequences
*   `seqfu bases` to print the composition of DNA sequences
*   `seqfu qual` to check the quality scores of FASTQ files

And many more...


---

[Back to the Programme for this week](week_1__programme.md)