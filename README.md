# Reproducible Sanger Analysis Workflow

A reproducible workflow for targeted Sanger sequencing analysis, from raw `.ab1` chromatogram files to reference alignment, candidate variant identification, chromatogram confirmation, coordinate conversion, and report-ready figure preparation.

This repository uses an ABO blood group gene analysis as an example case, but the same logic can be adapted to other Sanger sequencing projects.

---

## 1. Project Overview

This workflow documents how to analyze Sanger sequencing data when the user has:

- Raw `.ab1` chromatogram files
- Corresponding `.seq` files
- A known target gene or suspected target gene
- A reference sequence
- A need to identify candidate variants and prepare report-ready figures

The example case in this repository focuses on the **ABO blood group gene**.

In this example, four read pairs were checked:

- `G2H_1F + G2H_2R`
- `G2H_3F + G2H_4R`
- `HCQ_1F + HCQ_2R`
- `HCQ_3F + HCQ_4R`

The main candidate finding in the example case was:

- `NG_006669.2 RefPos 24142 C>T`
- Converted to `ABO c.657C>T`
- Located in `exon 7`
- Supported by both `HCQ_3F` and `HCQ_4R` chromatograms

This result should be reported as a **candidate ABO variant**, not as a complete ABO allele assignment by itself.

---

## 2. Data Privacy Notice

Do **not** upload identifiable clinical information to a public GitHub repository.

Avoid uploading:

- Patient names
- Medical record numbers
- Hospital report screenshots
- Raw patient `.ab1` files
- Raw patient `.seq` files
- Private clinical reports
- Customer messages or chat records

If the repository is public, use only:

- De-identified screenshots
- Simulated example data
- Workflow documentation
- Public reference sequence information
- Report templates without patient identifiers

Recommended: keep the repository private while developing the workflow.

---

## 3. Reference Sequence

For the ABO example analysis, the reference sequence was:

```text
NCBI Reference Sequence: NG_006669.2
Gene: ABO
Full name: ABO, alpha 1-3-N-acetylgalactosaminyltransferase and alpha 1-3-galactosyltransferase
Reference type: RefSeqGene, LRG_792
Chromosome: 9q34.2
```

Reference FASTA source:

```text
https://www.ncbi.nlm.nih.gov/nuccore/NG_006669.2?report=fasta&to=42144
```

The FASTA file was saved as:

```text
reference/ABO_reference_NG006669.fasta
```

The NCBI GenBank record for `NG_006669.2` was also used to check exon and CDS coordinates.

---

## 4. Software and Tools

The workflow used the following software and web resources:

| Tool | Purpose |
|---|---|
| UGENE | Map raw Sanger `.ab1` reads to the reference sequence |
| 4Peaks | Inspect original `.ab1` chromatogram peaks and base quality |
| NCBI Nucleotide | Retrieve the reference sequence |
| NCBI GenBank feature table | Confirm exon and CDS coordinates |
| ISBT ABO allele table | Interpret ABO-related coding DNA variants |
| Microsoft Word / PowerPoint | Annotate chromatogram images for report figures |

Optional tools for future automation:

| Tool | Purpose |
|---|---|
| Galaxy | Automated AB1-to-consensus workflow |
| Biopython | Programmatic AB1 parsing and sequence handling |
| MAFFT / Clustal Omega | Multiple sequence alignment |
| EMBOSS seqret | Sequence format conversion |
| seqtk | FASTQ trimming and processing |
| BLAST | Confirm sequence identity against public databases |

---

## 5. Suggested Repository Structure

```text
sanger-analysis-workflow/
├── README.md
├── .gitignore
├── reference/
│   └── ABO_reference_NG006669.fasta
├── docs/
│   ├── workflow_notes.md
│   ├── coordinate_conversion_notes.md
│   └── interpretation_notes.md
├── figures/
│   ├── example_alignment_view.png
│   ├── example_forward_chromatogram.png
│   └── example_reverse_chromatogram.png
├── report_template/
│   └── sanger_analysis_report_template.docx
└── results/
    └── candidate_variant_summary.md
```

If real clinical data are used, keep them outside the public repository:

```text
private/
raw_data/
patient_info/
reports_private/
```

These folders should be ignored by Git.

---

## 6. Input Files

Each Sanger read usually has two related files:

| File type | Description | Use |
|---|---|---|
| `.ab1` | Original Sanger chromatogram file | Primary evidence for peak quality and base calling |
| `.seq` | Base-called text sequence exported from the chromatogram | Useful for BLAST and quick sequence checks |

The `.ab1` file is the primary evidence for final interpretation because it contains:

- Chromatogram peaks
- Base calls
- Quality scores
- Signal quality
- Possible mixed peaks

The `.seq` file is useful as an auxiliary sequence file but should not replace chromatogram inspection.

---

## 7. Workflow Summary

```text
Raw .ab1 files
↓
Organize reads by sample and direction
↓
Download target gene reference FASTA
↓
Map Sanger reads to reference in UGENE
↓
Inspect reference, consensus, forward read, and reverse read
↓
Identify candidate sequence differences
↓
Confirm candidate sites using .ab1 chromatograms in 4Peaks
↓
Convert genomic reference position to coding DNA coordinate
↓
Interpret candidate variant using a gene-specific database
↓
Prepare report-ready figure and caption
```

---

# Detailed Workflow

---

## Step 1. Prepare the Reference Sequence

For the ABO example:

1. Open NCBI Nucleotide.
2. Search for:

```text
NG_006669.2
```

3. Open the record:

```text
Homo sapiens ABO, alpha 1-3-N-acetylgalactosaminyltransferase and alpha 1-3-galactosyltransferase, RefSeqGene (LRG_792) on chromosome 9
```

4. Click **FASTA**.
5. Save the FASTA sequence as:

```text
reference/ABO_reference_NG006669.fasta
```

The FASTA file should begin with:

```text
>NG_006669.2 Homo sapiens ABO, alpha 1-3-N-acetylgalactosaminyltransferase and alpha 1-3-galactosyltransferase (ABO), RefSeqGene (LRG_792) on chromosome 9
```

For other genes, replace this step with the correct RefSeqGene, genomic reference, or transcript reference for the target gene.

---

## Step 2. Organize Sanger Reads

Group forward and reverse reads by sample and amplicon.

Example from the ABO analysis:

```text
G2H_1F + G2H_2R
G2H_3F + G2H_4R
HCQ_1F + HCQ_2R
HCQ_3F + HCQ_4R
```

Forward reads are usually labeled with `F`.

Reverse reads are usually labeled with `R`.

Recommended naming format:

```text
Sample_ReadDirection.ab1
Sample_ReadDirection.seq
```

Example:

```text
HCQ_3F.ab1
HCQ_3F.seq
HCQ_4R.ab1
HCQ_4R.seq
```

---

## Step 3. Optional BLAST Check Using `.seq` Files

If the target gene is uncertain, use the `.seq` sequence for BLAST.

Purpose:

- Confirm that the read belongs to the expected gene
- Check whether the sequence matches the expected organism
- Detect obvious contamination or wrong sample type

For the ABO example, the `.seq` reads matched ABO-related sequences, supporting the use of `NG_006669.2` as the reference.

This BLAST step is useful when the sample identity or target gene is not fully known.

---

## Step 4. Map Sanger Reads to Reference in UGENE

Open UGENE and select:

```text
Tools → Sanger Data Analysis → Map Sanger Reads to Reference
```

Use the following settings:

| Field | Selection |
|---|---|
| Reference | `ABO_reference_NG006669.fasta` |
| Reads | Raw `.ab1` files |
| Trimming quality threshold | 30 |
| Mapping minimum similarity | 80% |
| Read name in result alignment | Sequence name from file |
| Add to project | Checked |

Important:

```text
Use raw .ab1 files for UGENE Sanger mapping.
Do not use .seq files for the main chromatogram-based mapping step.
Do not manually reverse-complement reverse reads before this step.
```

Run each read pair separately:

```text
G2H_1F.ab1 + G2H_2R.ab1
G2H_3F.ab1 + G2H_4R.ab1
HCQ_1F.ab1 + HCQ_2R.ab1
HCQ_3F.ab1 + HCQ_4R.ab1
```

UGENE should display:

```text
Reference
Consensus
Forward read
Reverse read
Chromatogram peaks
```

---

## Step 5. Inspect the Alignment

A high-confidence candidate variant should meet the following pattern:

```text
Reference:    C
Forward read: T
Reverse read: T
```

This means:

- The reference base is `C`
- The forward read supports `T`
- The reverse read also supports `T`
- Both reads agree with each other
- Both reads differ from the reference

A low-confidence or ambiguous site may look like:

```text
Reference:    C
Forward read: C
Reverse read: T
```

This pattern is weaker because only one read supports the difference.

A site should not be considered high-confidence if:

- Only one read supports the change
- The site is near the beginning of the read
- The site is near the end of the read
- The chromatogram peak is weak
- The peak is overlapped or noisy
- UGENE only shows an ambiguous consensus code without clear read support

---

## Step 6. Interpret Ambiguous IUPAC Codes

UGENE may show ambiguous bases in the consensus sequence.

| Code | Meaning |
|---|---|
| R | A or G |
| Y | C or T |
| M | A or C |
| K | G or T |
| W | A or T |
| S | C or G |
| N | Unknown base |

These codes do **not** automatically indicate a real mutation.

They may be caused by:

1. True heterozygosity.
2. Low-quality chromatogram peaks.
3. Peak overlap.
4. Inconsistent forward and reverse reads.
5. Poor quality at the beginning or end of a Sanger read.

Ambiguous calls must be confirmed by inspecting the original `.ab1` chromatogram.

---

## Step 7. Inspect Candidate Sites in 4Peaks

Open the corresponding `.ab1` files in 4Peaks.

For each candidate site, record:

| Field | Example |
|---|---|
| Sample | HCQ |
| Read pair | 3F/4R |
| Reference coordinate | NG_006669.2 RefPos 24142 |
| Reference base | C |
| Forward read base | T |
| Forward read position | T261 |
| Forward read quality | 55 |
| Reverse read base | T |
| Reverse read position | T766 |
| Reverse read quality | 55 |
| Interpretation | High-confidence candidate C>T variant |

A site is more reliable when:

```text
Both forward and reverse reads support the same base.
The chromatogram peak is clear.
The quality score is high.
The site is not near the low-quality beginning or ending region of the read.
```

In the ABO example, both reads supported the `T` call at the candidate site with quality score 55.

---

## Step 8. Convert Genomic RefPos to Coding DNA Coordinate

UGENE reports positions relative to the genomic reference sequence.

In the ABO example, UGENE identified:

```text
NG_006669.2 RefPos 24142 C>T
```

To interpret the site using ABO allele nomenclature, the genomic coordinate was converted to a coding DNA coordinate.

From the NCBI GenBank feature table for `NG_006669.2`:

```text
exon 7: 23860..29951
CDS exon 7 segment: 23860..24550
```

The candidate site:

```text
RefPos 24142
```

falls within exon 7 and within the CDS segment.

CDS segments before exon 7:

| CDS segment | Length |
|---|---:|
| 5026..5053 | 28 |
| 18047..18116 | 70 |
| 18841..18897 | 57 |
| 20349..20396 | 48 |
| 22083..22118 | 36 |
| 22673..22807 | 135 |

Total CDS length before exon 7:

```text
28 + 70 + 57 + 48 + 36 + 135 = 374
```

Position of RefPos 24142 within the exon 7 CDS segment:

```text
24142 - 23860 + 1 = 283
```

Coding DNA position:

```text
374 + 283 = 657
```

Therefore:

```text
NG_006669.2 RefPos 24142 C>T
= ABO c.657C>T
```

This site is located in exon 7.

---

## Step 9. Interpret the Candidate Variant

For the ABO example:

```text
NG_006669.2 RefPos 24142 C>T
= ABO c.657C>T
= Exon 7
```

Both forward and reverse reads supported the `T` call with high-quality chromatogram peaks.

This result should be reported as a **candidate ABO variant**.

However, complete ABO allele interpretation requires checking additional ABO-defining sites.

A single candidate variant is not sufficient to assign a complete ABO allele.

---

# Example Case Results: ABO Sanger Analysis

---

## Example 1. G2H_1F/2R

`G2H_1F/2R` successfully mapped to the ABO reference sequence `NG_006669.2`.

UGENE flagged several ambiguous bases, but most were located in low-quality or locally overlapped chromatogram regions, or were not consistently supported by both forward and reverse reads.

Result:

```text
No high-confidence variant was identified in the inspected G2H_1F/2R region.
```

---

## Example 2. G2H_3F/4R

`G2H_3F/4R` successfully mapped to `NG_006669.2` around RefPos 24100–24190.

A small number of ambiguous bases were flagged by UGENE, but no site showed clear and consistent support from both reads.

Result:

```text
No confirmed variant was identified in the inspected G2H_3F/4R region.
```

---

## Example 3. HCQ_1F/2R

`HCQ_1F/2R` successfully mapped to the ABO reference sequence `NG_006669.2` around RefPos 22629.

Although UGENE flagged multiple ambiguous bases, no site showed clear and consistent support from both forward and reverse reads.

Result:

```text
No high-confidence variant was identified in the inspected HCQ_1F/2R region.
```

---

## Example 4. HCQ_3F/4R

`HCQ_3F/4R` successfully mapped to the ABO RefSeqGene reference sequence `NG_006669.2`.

A candidate C>T substitution was observed at:

```text
NG_006669.2 RefPos 24142
```

At this position:

```text
Reference base: C
HCQ_3F: T
HCQ_4R: T
```

Chromatogram confirmation:

```text
HCQ_3F: T261, quality score 55
HCQ_4R: T766, quality score 55
```

Coordinate conversion:

```text
NG_006669.2 RefPos 24142 C>T
= ABO c.657C>T
= Exon 7
```

Interpretation:

```text
This site was recorded as a high-confidence candidate ABO c.657C>T variant in the HCQ sample.
However, a single confirmed site is not sufficient to assign a complete ABO allele.
Additional ABO-defining sites should be checked before final allele interpretation.
```

---

## Candidate Variant Summary Table

| Sample | Read pair | RefSeq coordinate | cDNA coordinate | Exon | Ref base | Read base | Read evidence | Interpretation |
|---|---|---:|---|---:|---|---|---|---|
| G2H | 1F/2R | Inspected region | Not applicable | Not assigned | Not applicable | Not applicable | No consistent forward/reverse variant | No high-confidence variant |
| G2H | 3F/4R | Around 24100–24190 | Not applicable | Exon 7 region | Not applicable | Not applicable | No consistent forward/reverse variant | No confirmed variant |
| HCQ | 1F/2R | Around 22629 | Not applicable | Exon 6 region | Not applicable | Not applicable | No consistent forward/reverse variant | No high-confidence variant |
| HCQ | 3F/4R | 24142 | c.657C>T | 7 | C | T | HCQ_3F T261 Q55; HCQ_4R T766 Q55 | High-confidence candidate variant |

---

## Figure Caption Template

### English

```text
Figure 1. Candidate ABO c.657C>T variant in the HCQ sample. The site corresponds to NG_006669.2 RefPos 24142. The reference base is C, while both HCQ_3F and HCQ_4R reads show T at the same position. The HCQ_3F chromatogram shows T261 with a quality score of 55, and the HCQ_4R chromatogram shows T766 with a quality score of 55.
```

### Chinese

```text
图 1  HCQ 样本 ABO Exon 7 候选变异 c.657C>T。该位点对应 NG_006669.2 RefPos 24142。参考序列为 C，而 HCQ_3F 与 HCQ_4R 双向 reads 均读为 T。HCQ_3F 峰图对应 T261，质量值为 55；HCQ_4R 峰图对应 T766，质量值为 55。
```

---

## Final Summary

In the inspected Sanger sequencing regions, the G2H sample did not show any high-confidence variant supported by both forward and reverse reads.

The HCQ sample showed one high-confidence candidate variant in the 3F/4R read pair.

This variant was located at:

```text
NG_006669.2 RefPos 24142
```

and corresponds to:

```text
ABO c.657C>T
```

in exon 7.

Both forward and reverse reads supported the `T` call with high-quality chromatogram peaks.

This result should be reported as a candidate ABO variant.

Complete ABO allele interpretation requires checking additional ABO-defining sites.

---

## Reproducibility Checklist

To make this workflow reproducible:

- [ ] Keep raw `.ab1` files unchanged.
- [ ] Store the exact reference FASTA file used.
- [ ] Record the reference accession number and version.
- [ ] Record UGENE mapping settings.
- [ ] Save screenshots of alignment views.
- [ ] Save screenshots of chromatogram peaks.
- [ ] Record both genomic coordinates and cDNA coordinates.
- [ ] Keep a table of all inspected candidate sites.
- [ ] Document ambiguous sites that were excluded.
- [ ] Record why each excluded site was not interpreted as a confirmed variant.
- [ ] Avoid uploading identifiable clinical data to public repositories.

---

## Limitations

1. Sanger reads cover only targeted regions, not the entire gene.
2. Ambiguous consensus calls require manual chromatogram review.
3. Low-quality read ends should not be used for final variant interpretation.
4. A single candidate variant is not sufficient for complete allele assignment.
5. Genomic RefSeq coordinates must be converted to coding DNA coordinates before allele interpretation.
6. Interpretation depends on the quality of forward and reverse read support.
7. Public repositories should not include patient-identifiable sequencing data.

---

## How to Reuse This Workflow

For another targeted Sanger sequencing project:

1. Identify the correct reference sequence.
2. Download the reference FASTA.
3. Organize forward and reverse `.ab1` files.
4. Map raw `.ab1` reads to the reference in UGENE.
5. Inspect forward/reverse read agreement.
6. Confirm candidate variants in chromatogram software.
7. Convert reference coordinates to transcript or coding DNA coordinates.
8. Interpret candidate variants using the appropriate gene or allele database.
9. Prepare report-ready figures with arrows and labels.
10. Record limitations and avoid over-interpreting single-site results.

---

## References

1. NCBI Reference Sequence: `NG_006669.2`. Homo sapiens ABO RefSeqGene sequence.
2. ISBT. Names for ABO (ISBT 001) blood group alleles.
3. Galaxy Training Network. Clean and manage Sanger sequences from raw files to aligned consensus.
4. UGENE documentation. Sanger reads mapping to reference.
5. 4Peaks chromatogram viewer documentation.
