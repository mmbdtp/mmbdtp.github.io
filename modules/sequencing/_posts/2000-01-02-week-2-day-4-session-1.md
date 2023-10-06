---
title: Assessing Read Quality, Trimming and Filtering
---


# Assessing Read Quality, Trimming and Filtering

Today we will work through the following tutorials, kindly provided by the [Data Carpentry](https://datacarpentry.org) initiative.

1. [Background and metadata](https://datacarpentry.org/wrangling-genomics/instructor/01-background.html)
2. [Assessing Read Quality](https://datacarpentry.org/wrangling-genomics/instructor/02-quality-control.html)
3. [Trimming and Filtering](https://datacarpentry.org/wrangling-genomics/instructor/03-trimming.html)

---

Once we have finished that, we can have some fun with `biomojify`:
 
- [https://github.com/fastqe/biomojify](https://github.com/fastqe/biomojify)

Install using pip

```
pip install biomojify 
```
The create a test fasta file

```
cat > test.fasta <<EOF
>SEQUENCE_1
ATAGTCAGTACGTAGTCGATGCTAGCTAGTAGGGGGGCTGATGATGTAGCTCGATCGTACGTACGTACGCTGAGTCAGTG
CACGTACGCTGCATGCCNTAGNCTAAAAGNCTANGCTAGCNTANGCTGACTNAGNTGACTGCNTCGTCGNATCATGTACG
TAGCGAGCTTTTTTTAGTGTACGTAGTACTACCCCCCCCCGTACGTACGTACGTCACGTGCTGACTNNNACGATCGTAGT
AGCTGACTGATGCT
EOF
```

Then run biomojify and look at the output!

```
biomojify fasta test.fasta 
```

Bonkers, but nice if you like emojis or fruit!!

Have a great weeekend!
