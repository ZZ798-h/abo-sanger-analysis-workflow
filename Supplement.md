## Supplement: G2H B(A)02/O02 Local Verification and HCQ Candidate Variant Review

### Purpose of This Supplement

This supplement updates the original Sanger analysis workflow to reflect the final interpretation strategy used in the ABO example case.

The project includes two different analytical purposes:

1. G2H sample: known-genotype local verification  
2. HCQ sample: candidate variant review

The G2H sample was reported by the transfusion research laboratory as:

    B(A)02/O02

Therefore, G2H was not interpreted as a blind mutation-screening case. Instead, the available G2H Sanger reads were used to locally verify whether the covered sites were consistent with the reported B(A)02/O02 genotype.

The HCQ sample was reviewed for candidate variants. One candidate site was identified:

    NG_006669.2 RefPos 24142 C>T
    = ABO c.657C>T
    = exon 7

This HCQ site was supported by both HCQ_3F and HCQ_4R reads.

---

### Updated Interpretation Strategy

The analysis used an allele-defined comparison approach.

For G2H, the reported genotype was:

    B(A)02/O02

For local comparison:

- B(A)02 was interpreted as ABO*BA.02.
- O02 was represented locally by ABO*O.02.01.
- The expected base at each ISBT-defined position was determined for both alleles.
- If the two alleles had the same expected base, the site was expected to show a consistent single-base pattern.
- If the two alleles had different expected bases, the site was expected to show a heterozygous pattern.

This method should be described as:

    ISBT allele-defined variant comparison

It should not be described as complete independent ABO allele assignment.

---

### G2H Local Verification Summary

The available G2H reads covered only part of the ABO gene.

The following G2H read pairs were reviewed:

- G2H_1F + G2H_2R
- G2H_3F + G2H_4R

The G2H_3F/4R reads covered only up to approximately:

    NG_006669.2 RefPos 24230

Therefore, several downstream BA.02/O.02.01-defining sites were not assessable in the current dataset.

---

### G2H Evaluated Sites

| Site | RefPos | Reference | BA.02 | O.02.01 | Expected G2H pattern | Observed G2H evidence | Interpretation |
|---|---:|---|---|---|---|---|---|
| c.297A>G | 22730 | A | G | G | G/G | G2H_1F C; G2H_2R G | Inconsistent; not confirmed |
| c.526C>G | 24011 | C | G | G | G/G | one read C; one read G | Inconsistent; not confirmed |
| c.657C>T | 24142 | C | T | C | C/T | G2H_3F T; G2H_4R C | Consistent with expected C/T pattern |
| c.700C>G | 24185 | C | G | C | C/G | G2H_3F C; G2H_4R C | Not supported |
| c.703G>A | 24188 | G | A | G | A/G | G2H_3F G; G2H_4R A | Consistent in UGENE alignment |
| c.796C>A | 24281 | C | A | C | A/C | Not covered | Not assessable |
| c.802G>A | 24287 | G | G | A | G/A | Not covered | Not assessable |
| c.803G>C | 24288 | G | C | G | C/G | Not covered | Not assessable |
| c.930G>A | 24415 | G | A | G | A/G | Not covered | Not assessable |

---

### G2H Interpretation

The G2H sample showed partial local support for the reported B(A)02/O02 genotype.

Sites consistent with the expected BA.02/O.02.01 pattern:

- c.657C>T
- c.703G>A

Sites not clearly supported or inconsistent:

- c.297A>G
- c.526C>G
- c.700C>G

Sites not covered by current G2H reads:

- c.796C>A
- c.802G>A
- c.803G>C
- c.930G>A

Therefore, the G2H result should be reported as:

    Local supporting evidence for the reported B(A)02/O02 genotype.
    Not a complete independent ABO allele assignment.

---

### G2H Figure Labels

#### Figure 1: G2H c.703G>A

Because the 4Peaks raw chromatogram coordinate was not fully matched to the UGENE reference coordinate for this site, Figure 1 should be described as a UGENE alignment result rather than chromatogram-confirmed evidence.

Recommended figure caption:

图 1  G2H 样本 Exon 7 位点 c.703G>A 的 UGENE 比对结果  
Figure 1  UGENE alignment result at Exon 7 c.703G>A in the G2H sample

Recommended labels:

- c.703G>A（G2H_3F：G）
- c.703G>A（G2H_4R：A）

Interpretation:

    G2H_3F showed G.
    G2H_4R showed A.
    This was consistent with the expected A/G pattern in UGENE alignment.

---

#### Figure 2: G2H c.657C>T

This site was interpreted as a C/T heterozygous pattern consistent with the reported BA.02/O.02.01 genotype combination.

Recommended figure caption:

图 2  G2H 样本 Exon 7 位点 c.657C>T 的 C/T 杂合峰图  
Figure 2  C/T heterozygous chromatogram pattern at Exon 7 c.657C>T in the G2H sample

Recommended labels:

- c.657C>T（正测，G2H_3F：T）
- c.657C>T（反测，G2H_4R：C）

Interpretation:

    BA.02 expected base: T
    O.02.01 expected base: C
    Expected G2H pattern: C/T
    Observed G2H pattern: G2H_3F T; G2H_4R C

This site can be used as local supporting evidence for the reported B(A)02/O02 genotype.

---

### HCQ Candidate Variant Summary

The HCQ sample was reviewed as a candidate variant case.

The following read pairs were reviewed:

- HCQ_1F + HCQ_2R
- HCQ_3F + HCQ_4R

HCQ_1F/2R did not show a high-confidence confirmed variant in the inspected exon 6 region.

HCQ_3F/4R showed a candidate C>T substitution at:

    NG_006669.2 RefPos 24142

This coordinate corresponds to:

    ABO c.657C>T
    exon 7

Observed evidence:

    Reference base: C
    HCQ_3F: T
    HCQ_4R: T
    HCQ_3F: T261, quality score 55
    HCQ_4R: T766, quality score 55

Interpretation:

    High-confidence candidate ABO c.657C>T variant in the HCQ sample.
    Not sufficient alone for complete ABO allele assignment.

---

### HCQ Figure Label

#### Figure 3: HCQ c.657C>T

Recommended figure caption:

图 3  HCQ 样本 Exon 7 候选突变 c.657C>T  
Figure 3  Candidate Exon 7 mutation c.657C>T in the HCQ sample

Recommended labels:

- c.657C>T（正测，HCQ_3F：T）
- c.657C>T（反测，HCQ_4R：T）

Interpretation:

    Both HCQ_3F and HCQ_4R supported the T call.
    The reference base was C.
    This site was recorded as a high-confidence candidate c.657C>T variant.

---

### Important Difference Between G2H and HCQ at RefPos 24142

Both G2H and HCQ were checked at the same genomic coordinate:

    NG_006669.2 RefPos 24142
    = ABO c.657C>T
    = exon 7

However, the interpretation is different.

For G2H:

    Reported genotype: B(A)02/O02
    BA.02 expected base: T
    O.02.01 expected base: C
    Expected pattern: C/T
    Observed: G2H_3F T; G2H_4R C
    Interpretation: consistent with expected heterozygous pattern

For HCQ:

    Reference base: C
    Observed: HCQ_3F T; HCQ_4R T
    Interpretation: high-confidence candidate C>T variant

Therefore:

    Same coordinate does not mean same interpretation.
    G2H c.657C>T is interpreted as local heterozygous support for B(A)02/O02.
    HCQ c.657C>T is interpreted as a candidate variant.

---

### Updated Final Summary

This supplement updates the workflow interpretation for the ABO Sanger analysis example.

The G2H sample had already been reported by the transfusion research laboratory as:

    B(A)02/O02

Therefore, G2H was analyzed using ISBT allele-defined variant comparison, not blind variant discovery.

Within the covered G2H read regions:

- c.657C>T was consistent with the expected C/T pattern.
- c.703G>A was consistent with the expected A/G pattern in UGENE alignment.
- c.297A>G, c.526C>G, and c.700C>G were not clearly supported or did not show the expected pattern.
- c.796C>A, c.802G>A, c.803G>C, and c.930G>A were not covered by the current G2H reads.

Therefore, the G2H result should be described as:

    Local supporting evidence for the reported B(A)02/O02 genotype.
    Not a complete independent ABO allele assignment.

The HCQ sample showed one high-confidence candidate variant:

    NG_006669.2 RefPos 24142 C>T
    = ABO c.657C>T
    = exon 7

Both HCQ_3F and HCQ_4R supported the T call with high-quality chromatogram evidence.

The HCQ result should be described as:

    High-confidence candidate ABO c.657C>T variant.
    Not a complete ABO allele assignment by itself.

---

### Suggested Commit Message

Use the following GitHub commit message:

    Add supplement for G2H B(A)02/O02 verification and HCQ candidate variant
