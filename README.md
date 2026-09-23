# Somatic Variant Analysis of CD56+ CTCs in SCLC

An educational reproduction of a whole-exome sequencing workflow for the public CD56+ circulating tumour cell sample **ERR6473446** from a small-cell lung cancer study.

This repository documents the quality-control reports, intermediate summaries, annotated candidate variants, and final internship report produced during the analysis. It is intended to demonstrate practical experience with cloud-based NGS processing—not to make an independent clinical claim.

## Dataset

| Field | Value |
| --- | --- |
| Sample | ERR6473446 |
| Material | CD56+ circulating tumour cells |
| Assay | Whole-exome sequencing |
| Reference genome | GRCh38 |
| Source | European Nucleotide Archive / SRA |
| Analysis setting | Tumour-only; no matched-normal sample was available |

The sample and biological context originate from the source study. Any interpretation in this repository should be read alongside that study and the limitations below.

## Workflow

The analysis was run in Ubuntu using AWS EC2/S3 and containerised bioinformatics tools.

1. **Acquisition and quality control** — SRA Toolkit, FastQC, and MultiQC.
2. **Alignment** — BWA-MEM against GRCh38.
3. **BAM processing** — SAMtools plus GATK read groups, duplicate marking, and base-quality recalibration.
4. **Candidate variant calling** — GATK Mutect2 in tumour-only mode, with population-resource filtering using gnomAD.
5. **Functional annotation** — SnpEff and SnpSift, followed by prioritisation of predicted HIGH- and MODERATE-impact calls.

## Repository contents

- `AdilSukumar_Somatic_Variant_Analysis_of_CD56+_Circulating_Tumor_Cells_in_SCLC.docx` — full project report
- `ERR6473446_1_fastqc.html` and `ERR6473446_2_fastqc.html` — raw-read quality-control reports
- `WES_Results/` — selected outputs from the analysis

This repository is an analysis record rather than a fully automated, one-command pipeline. Paths and resource versions should be adapted before attempting to reproduce the workflow.

## Interpretation

The workflow produced a set of **candidate** somatic variants for review, including calls in genes relevant to cancer biology. Functional-impact labels from SnpEff are computational predictions; they do not establish pathogenicity, causality, or clinical relevance. Candidate calls require manual review and, ideally, orthogonal validation.

## Limitations

- There was no matched-normal sample, so residual germline variants and sequencing artefacts may remain.
- This is a single public sample and does not support population-level conclusions.
- Variant annotations are predictions rather than experimental validation.
- No treatment recommendation or diagnostic conclusion should be drawn from this analysis.
- Comparisons with tissue biopsies or mutational signatures belong to the source study unless independently reproduced here.

## Tools

SRA Toolkit · FastQC · MultiQC · BWA-MEM · SAMtools · GATK · SnpEff · SnpSift · Docker · AWS EC2/S3

## Project context

Completed by **Adil Sukumar** during a bioinformatics internship at Sequensolutions (2025–2026). The work provided hands-on experience with WES data, Linux-based workflows, cloud infrastructure, and the care required when interpreting noisy biological data.

## License

See [LICENSE](LICENSE) for the repository license. Data access and reuse remain subject to the terms of the original archive and study.
