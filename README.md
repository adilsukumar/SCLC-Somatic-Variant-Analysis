# Somatic Variant Analysis of CD56+ Circulating Tumor Cells in SCLC

## Project Overview
This project focuses on the genomic profiling of **Small Cell Lung Cancer (SCLC)** through **Liquid Biopsy** targeting **CD56 (NCAM1)+** Circulating Tumor Cells (CTCs). Using a custom bioinformatics pipeline, we processed raw Whole Exome Sequencing (WES) data (Sample [ERR6473446](https://www.ebi.ac.uk/ena/browser/view/ERR6473446)) to identify somatic mutations and compare them against traditional solid tissue biopsies.

The goal was to evaluate how CTCs capture tumor heterogeneity and evolutionary shifts in mutational signatures (e.g., from tobacco-related damage to acquired evolutionary mutations).

## Key Findings
* **Higher Mutational Burden:** CD56+ CTCs captured a significantly higher mutation load (up to 290 mutations/Mb) compared to localized tissue biopsies (as low as 4.6 mutations/Mb).
* **Genetic Diversity:** CTCs provide a "sum" of all metastases, offering a more comprehensive view of the SCLC landscape than necrotic tissue biopsies.
* **Driver Mutations:** Validated biallelic inactivation of `TP53` (Indel/Frameshift) and `RB1` (Nonsense), alongside alterations in epigenetic regulators like `CREBBP` and `EP300`.
* **Evolutionary Shift:** Observed a transition from smoking-related `C>A` transversions in tissue to `C>T` transitions in CTCs, indicating ongoing tumor evolution.

## Bioinformatics Pipeline
The analysis was performed in a Linux environment using the following tools:

1.  **Data Acquisition:** SRA Toolkit (`prefetch`, `fasterq-dump`)
2.  **Quality Control:** FastQC, MultiQC
3.  **Alignment:** BWA-MEM (mapped to GRCh38)
4.  **BAM Refinement:** Samtools (sort, index), GATK (`AddOrReplaceReadGroups`, `MarkDuplicates`, `BQSR`)
5.  **Variant Calling:** GATK Mutect2 (Tumor-Only mode with gnomAD resource)
6.  **Annotation & Filtering:** SnpEff (functional impact) and SnpSift (high/moderate impact filtration)

## Repository Structure
* `data/`: (Placeholder for sample metadata or links to raw data)
* `scripts/`: Shell scripts used for alignment and variant calling.
* `results/`: 
    * `tumor.high_mod.vcf`: Filtered VCF containing high and moderate impact variants.
    * `tumor.coding_effect.vcf`: Variants specifically altering protein-coding sequences.
* `plots/`: Visualizations including Mutational Signature shifts and TMB comparisons.

## Requirements
* Ubuntu/Linux
* Docker
* SRA Toolkit
* BWA, Samtools, GATK4
* SnpEff & SnpSift

## References
* Ricordel et al. (2023). *Genomic characteristics and clinical significance of CD56+ circulating tumor cells in small cell lung cancer.* [PMC9984363](https://pmc.ncbi.nlm.nih.gov/articles/PMC9984363/)
* Broad Institute. GATK Best Practices.
