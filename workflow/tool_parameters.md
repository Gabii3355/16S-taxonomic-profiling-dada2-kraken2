# Tool parameters

## 1. Analysis overview

This document records the tools, parameters and reference resources used in
the comparative taxonomic profiling of a 16S rRNA amplicon sequencing sample.

The analysis was performed in Galaxy using two approaches:

1. DADA2 for denoising, ASV inference and taxonomic assignment
2. Kraken2 with Bracken for k-mer-based taxonomic classification and
   abundance estimation

Taxonomic profiles were visualized using Krona.

---

## 2. Input data

| Parameter | Value |
|---|---|
| Sample accession | SRR13569534 |
| Sequencing platform | Illumina |
| Read type | Paired-end |
| Target | 16S rRNA gene |
| Variable regions | V3–V4 |
| Initial number of read pairs | 67,311 |
| Input format | FASTQ |
| Forward reads | [add original filename] |
| Reverse reads | [add original filename] |

Raw sequencing files are not included in this repository due to their size.

---

## 3. Galaxy environment

| Parameter | Value |
|---|---|
| Galaxy instance | [add Galaxy server URL or name] |
| Analysis date | [add date] |
| Galaxy history name | [add history name] |
| Workflow file | [add workflow filename, if exported] |

Tool versions should be added from the Galaxy history before publication.

---

# DADA2 workflow

## 4. Quality assessment

Quality profiles were generated separately for forward and reverse reads.

The forward reads maintained relatively high quality across most of their
length. The reverse reads showed a stronger decline in quality after
approximately 180–220 sequencing cycles.

Based on these profiles, the reverse reads were trimmed more aggressively
than the forward reads.

---

## 5. Filtering and trimming

Galaxy tool:

`dada2: filterAndTrim`

| Parameter | Forward reads | Reverse reads |
|---|---:|---:|
| Truncation length (`truncLen`) | 280 bp | 220 bp |
| Maximum expected errors (`maxEE`) | 2 | 3 |
| Maximum ambiguous bases (`maxN`) | 0 | 0 |
| Truncation quality (`truncQ`) | 2 | 2 |
| Trim from start (`trimLeft`) | 0 | 0 |
| Trim from end (`trimRight`) | 0 | 0 |
| Minimum read length (`minLen`) | 200 bp | 200 bp |

### Filtering result

| Metric | Value |
|---|---:|
| Initial read pairs | 67,311 |
| Read pairs retained | 34,553 |
| Percentage retained | approximately 51% |

The truncation lengths were selected to remove low-quality read tails while
preserving sufficient overlap for successful paired-end read merging.

---

## 6. Error modelling and ASV inference

| Parameter | Value |
|---|---|
| DADA2 version | [add from Galaxy history] |
| Error learning parameters | [add from Galaxy history] |
| Denoising parameters | [add from Galaxy history] |
| Pooling mode | [add if available] |
| Number of threads | [add if available] |

Only parameters that can be confirmed from the Galaxy history should be added.

---

## 7. Paired-read merging

Galaxy tool:

`DADA2 mergePairs`

| Parameter | Value |
|---|---|
| Minimum overlap | [add from Galaxy history] |
| Maximum mismatches | [add from Galaxy history] |
| Other non-default parameters | [add or write "Galaxy defaults"] |

The merged reads showed several dozen matching bases in the overlap region.
No mismatches or indels were observed in the inspected merging results.

---

## 8. Chimera removal

| Parameter | Value |
|---|---|
| Galaxy tool | [add exact tool name] |
| DADA2 version | [add version] |
| Chimera detection method | [add from Galaxy history] |
| Consensus method | [add if applicable] |
| Other parameters | [add from Galaxy history] |

---

## 9. Taxonomic assignment

| Parameter | Value |
|---|---|
| Galaxy tool | [add exact tool name] |
| Reference database | [add database name] |
| Database version | [add database version] |
| Minimum bootstrap confidence | [add if available] |
| Taxonomic ranks reported | Class and genus |
| Other parameters | [add from Galaxy history] |

The database name and version are important because taxonomic assignments
depend on the reference taxonomy used.

---

# Kraken2 and Bracken workflow

## 10. Kraken2 classification

| Parameter | Value |
|---|---|
| Kraken2 version | [add from Galaxy history] |
| Input type | Paired-end reads |
| Reference database | [add exact database name] |
| Database version or build date | [add if available] |
| Confidence threshold | [add from Galaxy history] |
| Minimum hit groups | [add from Galaxy history] |
| Report zero-count taxa | [Yes/No] |
| Number of threads | [add if available] |
| Other non-default parameters | [add from Galaxy history] |

Kraken2 classified reads using exact k-mer matches against the selected
reference database.

---

## 11. Bracken abundance estimation

| Parameter | Value |
|---|---|
| Bracken version | [add from Galaxy history] |
| Kraken2 database | [same database as above] |
| Read length | [add value] |
| Taxonomic level | Class and genus |
| Minimum number of reads | [add from Galaxy history] |
| Abundance threshold | [add if applicable] |
| Other parameters | [add from Galaxy history] |

Bracken was used to refine Kraken2 classifications and estimate relative
taxonomic abundances.

---

# Visualization

## 12. Krona visualization

| Parameter | DADA2 | Kraken2/Bracken |
|---|---|---|
| Input data | Taxonomic abundance table | Bracken abundance report |
| Taxonomic levels visualized | Class and genus | Class and genus |
| Krona version | [add version] | [add version] |
| Minimum abundance threshold | [add if used] | [add if used] |
| Output format | Interactive HTML / static image | Interactive HTML / static image |

The visualizations were used to compare the relative abundances identified
by both taxonomic profiling methods.

---

## 13. Parameters that still require confirmation

The following information was not recorded in the original report and should
be checked in the Galaxy history:

- Galaxy instance and analysis date
- exact tool versions
- DADA2 taxonomy reference database and version
- DADA2 error-learning parameters
- paired-read merging parameters
- chimera-removal settings
- Kraken2 database name and build
- Kraken2 confidence and classification parameters
- Bracken read length and minimum-read threshold
- Krona version
- parameters that remained at their Galaxy default values

Unknown parameters should be marked as `not recorded` rather than estimated.