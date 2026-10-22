---
title: FASTQ Format
---

FASTQ is the standard file format for storing sequencing reads. Each read is stored as four lines: a header, the sequence, a separator, and a quality string. Understanding this format — and what the quality string actually means — is the foundation for everything else we do in bioinformatics QC.

> 💡 **TL;DR** A FASTQ file is a FASTA file with quality scores attached. The quality string encodes the probability of error for each base. Higher quality scores mean fewer sequencing errors.

---

## Anatomy of a FASTQ record

Every read in a FASTQ file looks like this:

<div style="font-family: monospace; background: #f6f8fa; border: 1px solid #d0d7de; border-radius: 6px; padding: 1.2em; line-height: 2em; overflow-x: auto;">
  <span style="color:#6a737d;">Line 1 — Header</span><br>
  <span style="color:#0550ae; font-weight:bold;">@SRR34548621.1 some description here</span><br>
  <span style="color:#6a737d;">Line 2 — Sequence</span><br>
  <span style="color:#1a7f37; font-weight:bold;">ACGTACGTACGTACGTACGTACGT</span><br>
  <span style="color:#6a737d;">Line 3 — Separator (always a +)</span><br>
  <span style="color:#953800;">+</span><br>
  <span style="color:#6a737d;">Line 4 — Quality string (one character per base)</span><br>
  <span style="color:#8250df; font-weight:bold;">IIIIIIIIIIII?88888&lt;&lt;&lt;&lt;&lt;&lt;</span>
</div>

Each record (sequence) will be stored in exactly 4 lines:

* The header starts with `@` and carries the read name and (optionally) metadata. The quality string is always exactly the same length as the sequence — one character per base.
* The sequence line contains the nucleotide sequence of the read.
* The separator line is always a `+` and may optionally repeat the header.
* The quality string encodes the *Phred quality score* for each base in the sequence.

---

## Phred quality scores

The quality string looks like gibberish, but each character encodes a **Phred quality score (Q)** — a number that tells you how likely that base call is wrong.

The formula is:

> **Q = −10 × log₁₀(P)**  where P = probability the base is wrong

So a base with a 1-in-100 chance of being wrong has Q = 20. A base with a 1-in-1000 chance has Q = 30. The higher the number, the more confident the sequencer was.

| Phred score | Error probability | 1 error in… | Accuracy |
|:-----------:|:-----------------:|:-----------:|:--------:|
| Q10         | 10%               | 10 bases    | 90%      |
| Q20         | 1%                | 100 bases   | 99%      |
| Q30         | 0.1%              | 1,000 bases | 99.9%    |
| Q40         | 0.01%             | 10,000 bases| 99.99%   |

**Q30 is the conventional threshold** for "good quality" Illumina data. Q20 is often used as a lower bound for trimming.

### How scores are encoded

Phred scores are stored as single ASCII characters, not as numbers, to keep the file compact. The encoding is:

> **ASCII character = Phred score + 33**

So Q score 30 → 30 + 33 = 63 → the character `?`. Score 40 → 73 → `I`. Score 0 → 33 → `!`.

<div style="font-family: monospace; background: #f6f8fa; border: 1px solid #d0d7de; border-radius: 6px; padding: 1.2em; overflow-x: auto;">
  <div style="margin-bottom: 0.5em; color: #6a737d; font-size: 0.9em;">Each character maps to a Q score:</div>
  <div style="display: flex; gap: 0; flex-wrap: wrap;">
    <!-- low quality -->
    <div style="text-align:center; margin: 2px;">
      <div style="background:#d73a49; color:white; padding:6px 8px; border-radius:4px; font-size:1.1em;">!</div>
      <div style="font-size:0.75em; color:#6a737d; margin-top:2px;">Q0</div>
    </div>
    <div style="text-align:center; margin: 2px;">
      <div style="background:#e05252; color:white; padding:6px 8px; border-radius:4px; font-size:1.1em;">&amp;</div>
      <div style="font-size:0.75em; color:#6a737d; margin-top:2px;">Q5</div>
    </div>
    <div style="text-align:center; margin: 2px;">
      <div style="background:#e8935a; color:white; padding:6px 8px; border-radius:4px; font-size:1.1em;">+</div>
      <div style="font-size:0.75em; color:#6a737d; margin-top:2px;">Q10</div>
    </div>
    <div style="text-align:center; margin: 2px;">
      <div style="background:#e8c35a; color:white; padding:6px 8px; border-radius:4px; font-size:1.1em;">8</div>
      <div style="font-size:0.75em; color:#6a737d; margin-top:2px;">Q23</div>
    </div>
    <div style="text-align:center; margin: 2px;">
      <div style="background:#5aae61; color:white; padding:6px 8px; border-radius:4px; font-size:1.1em;">?</div>
      <div style="font-size:0.75em; color:#6a737d; margin-top:2px;">Q30</div>
    </div>
    <div style="text-align:center; margin: 2px;">
      <div style="background:#2e8b57; color:white; padding:6px 8px; border-radius:4px; font-size:1.1em;">F</div>
      <div style="font-size:0.75em; color:#6a737d; margin-top:2px;">Q37</div>
    </div>
    <div style="text-align:center; margin: 2px;">
      <div style="background:#1a7f37; color:white; padding:6px 8px; border-radius:4px; font-size:1.1em;">I</div>
      <div style="font-size:0.75em; color:#6a737d; margin-top:2px;">Q40</div>
    </div>
  </div>
  <div style="margin-top: 0.8em; font-size: 0.85em; color: #6a737d;">Red = poor quality → Yellow = acceptable → Green = good quality</div>
</div>


<div style="font-family: monospace; background: #f6f8fa; border: 1px solid #d0d7de; border-radius: 6px; padding: 1.2em; line-height: 1.8em; overflow-x: auto;">
<div style="display: flex; flex-wrap: wrap; gap: 0.6em; align-items: center; margin-bottom: 0.8em;">
<input id="phred-input" type="text" value="I?8#" placeholder="e.g. IIII?88#" autocomplete="off" spellcheck="false" style="font-family: monospace; font-size: 1em; padding: 0.3em 0.5em; border: 1px solid #d0d7de; border-radius: 6px; background: #ffffff; color: #8250df; font-weight: bold; width: 14em;">
<select id="phred-offset" style="font-family: monospace; font-size: 1em; padding: 0.3em 0.5em; border: 1px solid #d0d7de; border-radius: 6px; background: #ffffff; color: #24292f;">
<option value="33" selected>Phred+33 (Illumina 1.8+, Sanger)</option>
<option value="64">Phred+64 (Illumina 1.3-1.7)</option>
</select>
<button id="phred-button" type="button" style="font-family: monospace; font-size: 1em; padding: 0.3em 1em; border: 1px solid #d0d7de; border-radius: 6px; background: #0550ae; color: #ffffff; font-weight: bold; cursor: pointer;">Decode</button>
</div>
<div id="phred-output" style="white-space: pre; color: #24292f;"></div>
<script>
(function () {
  var input = document.getElementById("phred-input");
  var select = document.getElementById("phred-offset");
  var button = document.getElementById("phred-button");
  var output = document.getElementById("phred-output");
  function colour(q) {
    if (q >= 30) { return "#1a7f37"; }
    if (q >= 20) { return "#d97706"; }
    return "#cf222e";
  }
  function addRow(ch, text, colr) {
    var row = document.createElement("div");
    var c = document.createElement("span");
    c.textContent = ch;
    c.style.color = colr;
    c.style.fontWeight = "bold";
    var t = document.createElement("span");
    t.textContent = text;
    row.appendChild(c);
    row.appendChild(t);
    output.appendChild(row);
  }
  function decode() {
    output.textContent = "";
    var s = input.value;
    var offset = parseInt(select.value, 10);
    if (s.length === 0) {
      output.textContent = "Enter one or more quality characters.";
      return;
    }
    for (var i = 0; i !== s.length; i++) {
      var code = s.charCodeAt(i);
      var q = code - offset;
      if (q < 0) {
        addRow(s.charAt(i), "  ASCII " + code + "  invalid for this offset", "#cf222e");
      } else {
        var p = Math.pow(10, -q / 10);
        addRow(s.charAt(i), "  ASCII " + code + "  Q" + q + "  P(error) = " + p.toExponential(2), colour(q));
      }
    }
  }
  button.addEventListener("click", decode);
  input.addEventListener("keydown", function (e) {
    if (e.key === "Enter") { decode(); }
  });
  decode();
})();
</script>
</div>

---

## Quality expectations: Illumina vs Nanopore

The two platforms have very different error profiles, and that shapes everything about how you work with their data.

<div style="display: flex; gap: 1em; flex-wrap: wrap; margin: 1em 0;">

  <div style="flex: 1; min-width: 260px; border: 2px solid #0550ae; border-radius: 8px; padding: 1em;">
    <div style="font-weight: bold; font-size: 1.1em; color: #0550ae; margin-bottom: 0.5em;">Illumina</div>
    <div style="font-family: monospace; background: #f0f4ff; border-radius: 4px; padding: 0.6em; margin-bottom: 0.6em; font-size: 0.85em;">
      Typical read: 150 bp<br>
      <span style="color:#1a7f37;">IIIIIIIIIIIIIIIIIIIIIIIFFF??88+</span><br>
      Q:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
      <span style="color:#1a7f37;">40→37→30→23→10</span>
    </div>
    <ul style="margin: 0; padding-left: 1.2em; font-size: 0.9em;">
      <li>Short reads (75–300 bp)</li>
      <li>Per-base accuracy typically <strong>Q30–Q40</strong></li>
      <li>Quality drops toward the 3′ end (cycle degradation)</li>
      <li>Random substitution errors (not systematic)</li>
      <li>Q30 is the standard QC threshold</li>
    </ul>
  </div>

  <div style="flex: 1; min-width: 260px; border: 2px solid #953800; border-radius: 8px; padding: 1em;">
    <div style="font-weight: bold; font-size: 1.1em; color: #953800; margin-bottom: 0.5em;">Oxford Nanopore</div>
    <div style="font-family: monospace; background: #fff4f0; border-radius: 4px; padding: 0.6em; margin-bottom: 0.6em; font-size: 0.85em;">
      Typical read: 5,000–50,000 bp<br>
      <span style="color:#e08020;">8+88?88+88&amp;8+?88?+8&amp;88+?88</span><br>
      Q:&nbsp;<span style="color:#e08020;">fluctuates ~Q15–Q25 throughout</span>
    </div>
    <ul style="margin: 0; padding-left: 1.2em; font-size: 0.9em;">
      <li>Long reads (1 kb – 4 Mb)</li>
      <li>Per-base accuracy typically <strong>Q15–Q25</strong> (chemistry-dependent)</li>
      <li>Errors distributed across the whole read, not just the end</li>
      <li>Insertion/deletion errors more common than substitutions</li>
      <li>Q20 is a common threshold for recent chemistries (R10.4.1+)</li>
    </ul>
  </div>

</div>

> ⚠️ A Nanopore dataset with mean Q12 and an Illumina dataset with mean Q12 are very different problems. Illumina Q12 indicates severe degradation or a failed run. Nanopore Q12 was typical for older flow cells and is still usable for many applications (e.g. species-level metagenomic profiling).

### How accuracy has improved in Nanopore

Nanopore accuracy has improved substantially over successive chemistries and basecallers:

| Era                        | Typical Q   | Notes                              |
|----------------------------|:-----------:|------------------------------------|
| R9.4.1 (Guppy, fast mode)  | Q10–Q14     | Useful for long-range structure    |
| R9.4.1 (Guppy, sup mode)   | Q15–Q18     | Most historical datasets           |
| R10.4.1 (Dorado, sup mode) | Q20–Q25     | Current standard; rivals Illumina for variant calling |

---

## Inspecting a FASTQ file

You can peek at a FASTQ file directly, but remember that each record is 4 lines — so "the first 8 lines" gives you two reads:

```bash
zcat sample.fastq.gz | head -8
```

To count reads:

```bash
# Each read = 4 lines
zcat sample.fastq.gz | wc -l | awk '{print $1/4}'
```

To get a quick summary with **seqfu**:

```bash
seqfu stats sample.fastq.gz
```

- Output includes: number of reads, total bases, mean/min/max length, mean quality.

> 💡 `seqfu` handles gzip-compressed files natively — you do not need to decompress first.

---

## Questions

* ❓ Open a FASTQ file and find the quality string for the first read. What is the Q score of the first base? What is the Q score of the last base? Is there a trend?
* ❓ A read has a quality string that starts with `IIIIIIII` and ends with `!!!!!!`. What does that tell you about how the sequencer performed across the read?
* ❓ You have a Nanopore dataset with mean Q15 and an Illumina dataset with mean Q30. Which one has more errors per base? By how much?
* ❓ Why can't you just use the same QC thresholds for Illumina and Nanopore data?
