# Comparative 16S rRNA Taxonomic Profiling Using DADA2 and Kraken2
## Overview

This project presents a comparative analysis of 16S rRNA amplicon sequencing
data using two taxonomic profiling approaches: DADA2 and Kraken2 with Bracken.

The analysis was performed in Galaxy using paired-end Illumina reads from
sample SRR13569534, targeting the V3–V4 variable regions of the bacterial
16S rRNA gene.

The project focused on quality-based read trimming, taxonomic identification,
relative abundance visualization and comparison of results obtained with
ASV-based and k-mer-based methods.

## Objectives

- Evaluate the quality of forward and reverse sequencing reads
- Select trimming parameters based on quality profiles
- Perform ASV inference and taxonomic assignment using DADA2
- Perform k-mer-based taxonomic classification using Kraken2
- Estimate taxonomic abundance using Bracken
- Visualize community composition using Krona
- Compare the taxonomic profiles produced by both approaches
  
## Dataset

- Accession: SRR13569534
- Sequencing type: paired-end Illumina
- Target: bacterial 16S rRNA
- Variable regions: V3–V4
- Initial number of read pairs: 67,311
Raw FASTQ files are not included in this repository due to their size.

## Workflow
```mermaid
flowchart TD
    A[Paired-end Illumina FASTQ<br>SRR13569534] --> B[Sequence quality assessment]
    B --> C[Trimming and filtering]

    C --> D[DADA2 workflow]
    D --> E[Error modelling and denoising]
    E --> F[Paired-read merging]
    F --> G[Chimera removal]
    G --> H[ASV inference]
    H --> I[Taxonomic assignment]
    I --> J[DADA2 Krona visualization]

    C --> K[Kraken2 workflow]
    K --> L[k-mer-based classification]
    L --> M[Bracken abundance estimation]
    M --> N[Kraken2 Krona visualization]

    J --> O[Comparison of taxonomic profiles]
    N --> O

## Quality filtering

The forward reads maintained relatively high quality across most of their
length, while the reverse reads showed a stronger quality decline after
approximately 180–220 cycles.

Based on the quality profiles, the following truncation lengths were selected:

| Parameter | Forward | Reverse |
|---|---:|---:|
| Truncation length | 280 bp | 220 bp |
| Maximum expected errors | 2 | 3 |

Additional parameters included maxN = 0, truncQ = 2 and a minimum read
length of 200 bp.

After filtering, 34,553 of 67,311 read pairs were retained, corresponding
to approximately 51% of the original data. 
```
## Main results

Both methods identified Firmicutes/Clostridia and
Bacteroidetes/Bacteroidia as the dominant groups.

At class level:

| Taxonomic group | DADA2 | Kraken2/Bracken |
|---|---:|---:|
| Clostridia | 51% | 41% |
| Bacteroidia | 35% | 51% |

DADA2 produced a clearer genus-level profile based on denoised amplicon
sequence variants. Kraken2 reported more species-level assignments because
classification was based on k-mer matches against a genome reference database.

## Interpretation

The differences between DADA2 and Kraken2 can result from:

- different classification algorithms,
- different reference databases,
- differences in taxonomic naming,
- the limited species-level resolution of the V3–V4 region,
- different approaches to handling ambiguous assignments.

DADA2 was more directly interpretable for this 16S amplicon dataset,
whereas Kraken2 produced more detailed but more database-dependent
taxonomic labels.

## Limitations

- The analysis included only one biological sample.
- Results depend strongly on the selected reference databases.
- Species-level assignments based on the V3–V4 region should be interpreted cautiously.
- The workflow was executed in Galaxy rather than through a standalone workflow manager.
- No biological replicates or statistical comparisons between sample groups were available.
  
## Repository
```text
16S-taxonomic-profiling-dada2-kraken2/
│
├── README.md
│
├── report/
│   └── 16S_metagenomic_analysis_report.pdf
│
├── workflow/
│   ├── dada2_workflow.ga
│   ├── kraken2_workflow.ga
│   └── tool_parameters.md
│
├── results/
│   ├── quality_profiles/
│   │   ├── raw_forward.png
│   │   ├── raw_reverse.png
│   │   ├── trimmed_forward.png
│   │   └── trimmed_reverse.png
│   │
│   ├── krona/
│   │   ├── dada2_class_level.png
│   │   ├── dada2_genus_level.png
│   │   ├── kraken2_class_level.png
│   │   └── kraken2_genus_level.png
│   │
│   └── tables/
│       ├── dada2_taxonomic_abundance.tsv
│       ├── kraken2_taxonomic_abundance.tsv
│       └── class_level_comparison.csv
│
├── notebooks/
│   └── taxonomic_comparison.ipynb
│
└── data/
    └── README.md
```
