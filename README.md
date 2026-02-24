# 🧬 Somatic Variant Analysis of CD56+ CTCs in SCLC

[cite_start]This repository contains the complete bioinformatics analysis pipeline—from raw sequencing data to a prioritized somatic variant list—for the liquid biopsy sample **ERR6473446**[cite: 2, 167].

[cite_start]The project demonstrates an end-to-end workflow to identify significant genetic variants in **CD56+ Circulating Tumor Cells (CTCs)** from Small Cell Lung Cancer (SCLC) patients[cite: 10, 168]. [cite_start]This approach offers a "liquid window" into tumor heterogeneity and evolution that traditional solid biopsies often miss[cite: 149, 159].

---

## 1. Project Overview

### 🔬 About the Data
[cite_start]Raw Whole Exome Sequencing (WES) data was obtained from the European Nucleotide Archive (ENA)[cite: 2, 167].

| Field | Value |
| :--- | :--- |
| **Sample Accession** | [cite_start]ERR6473446 [cite: 2, 167] |
| **Marker Target** | [cite_start]CD56 (NCAM1) [cite: 10, 177] |
| **Cancer Type** | [cite_start]Small Cell Lung Cancer (SCLC) [cite: 5, 172] |
| **Strategy** | [cite_start]Whole Exome Sequencing (WES) [cite: 12, 179] |
| **Reference Study** | [cite_start]Ricordel et al., *Scientific Reports* (2023) [cite: 3, 164] |
| **Reference Genome** | [cite_start]GRCh38 (hg38) [cite: 23, 193] |

**Study Background:**
[cite_start]SCLC is a highly aggressive cancer characterized by rapid doubling time and early metastasis[cite: 6, 173]. [cite_start]Because traditional tissue biopsies are often necrotic or limited, this project focuses on **CD56+ Liquid Biopsy** to capture the full genetic diversity of the tumor landscape[cite: 9, 10, 176, 177].

---

## 2. Methodology & Pipeline

[cite_start]The analysis was performed in a **Linux (Ubuntu)** environment utilizing **AWS EC2** and **Docker** for containerized tool management[cite: 12, 91, 277].

### Step 1 — Data Acquisition & QC
* [cite_start]Downloaded raw reads using `prefetch` and `fasterq-dump` from the SRA Toolkit[cite: 15, 100, 182].
* [cite_start]Assessed read fidelity using **FastQC** and **MultiQC**, verifying a **Phred score > 28**[cite: 21, 101, 102, 287].

### Step 2 — Alignment & Post-Processing
* [cite_start]Mapped reads to **GRCh38** using **BWA-MEM** with 16 threads[cite: 23, 24, 105, 194].
* [cite_start]Converted SAM to **BAM**, followed by coordinate sorting and indexing via **Samtools**[cite: 26, 28, 107, 108].
* [cite_start]Used **GATK AddOrReplaceReadGroups** and **MarkDuplicates** to identify and flag PCR duplicates[cite: 30, 39, 111, 211].
* [cite_start]Performed **Base Quality Score Recalibration (BQSR)** to correct systematic errors in base quality[cite: 45, 112, 217].

### Step 3 — Somatic Variant Calling
* [cite_start]Identified somatic mutations using **GATK Mutect2** in Tumor-Normal mode[cite: 56, 114, 228].
* [cite_start]Utilized the **gnomAD** resource to filter out common population germline variants[cite: 57, 114, 229, 300].

### Step 4 — Functional Annotation & Filtering
* [cite_start]Predicted functional consequences using **SnpEff** (GRCh38.99 database)[cite: 68, 116, 247, 302].
* [cite_start]Filtered for **HIGH** and **MODERATE** impact variants using **SnpSift**[cite: 76, 120, 257, 309].
* [cite_start]Specifically isolated variants altering the protein-coding sequence (e.g., stop-gained, frameshifts)[cite: 81, 121, 264, 310].

---

## 3. Key Analytical Findings

[cite_start]The analysis confirmed a highly mutated and heterogeneous tumor profile, capturing the "sum" of metastases[cite: 123, 152].

### 📊 Variant Summary (Key Drivers)
| Impact Tier | Dominance (%) | Key Genes Affected |
| :--- | :--- | :--- |
| **Indel/Frameshift** | 85.7% | [cite_start]`TP53`, `CREBBP`, `EP300`, `COL22A1`, `PTEN`, `NOTCH3` [cite: 134, 372] |
| **Non-Sense** | 14.3% | [cite_start]`RB1` [cite: 136, 375] |

---

### 🔴 High-Impact Driver Mutations

#### Foundational Tumor Suppressors
| Gene | Variant Type | Biological Impact & Mechanism |
| :--- | :--- | :--- |
| **RB1** | Non-Sense | [cite_start]Abolishes cell-cycle brakes, triggering uncontrolled proliferation[cite: 146, 406]. |
| **TP53** | Frameshift | [cite_start]Nullifies p53 protein, preventing DNA repair and enabling tumor survival[cite: 146, 407]. |

#### Epigenetic & Signaling Regulators
| Gene | Variant Type | Biological Impact & Mechanism |
| :--- | :--- | :--- |
| **CREBBP** | Frameshift | [cite_start]Disrupts histone acetylation, causing silencing of tumor-suppressor genes[cite: 146, 407]. |
| **EP300** | Frameshift | [cite_start]Aborts protein synthesis, disabling chromatin remodeling[cite: 146, 407]. |
| **PTEN** | Frameshift | [cite_start]Induces signaling alterations in the PI3K pathway[cite: 146, 407]. |

#### Metastatic Priming
| Gene | Variant Type | Biological Impact & Mechanism |
| :--- | :--- | :--- |
| **COL22A1** | Frameshift | [cite_start]Destabilizes ECM, impairing tissue integrity to facilitate metastasis[cite: 146, 407]. |
| **NOTCH3** | Frameshift | [cite_start]Disrupts neuroendocrine differentiation, driving SCLC progression[cite: 146, 407]. |

---

## 4. Interpretation & Clinical Context

### Tumor Heterogeneity & Load
[cite_start]Comparative analysis showed that CD56+ CTCs consistently capture a massive mutation load, **20 to 30 times higher** than matched tissue biopsies[cite: 151, 449]. [cite_start]This confirms that liquid biopsy provides a more comprehensive view of the evolving SCLC landscape[cite: 129, 363].

### Mutational Signature Shift
* [cite_start]**Tissue Biopsy:** Massive spike in **C>A transversions**, representing the "smoking gun" of tobacco damage[cite: 141, 154, 452].
* [cite_start]**CD56+ CTCs:** Significant shift toward **C>T transitions**, indicating that circulating cells acquire new evolutionary mutations unrelated to the original smoking damage[cite: 141, 154, 452, 453].

---

## 5. Tools & Dependencies

| Tool | Purpose |
| :--- | :--- |
| **SRA Toolkit** | [cite_start]Raw data acquisition (`prefetch`, `fasterq-dump`) [cite: 100, 286] |
| **FastQC / MultiQC** | [cite_start]Quality control and reporting [cite: 101, 103, 288] |
| **BWA-MEM** | [cite_start]Read alignment to GRCh38 [cite: 105, 290] |
| **Samtools** | [cite_start]BAM conversion, sorting, and indexing [cite: 107, 108, 292] |
| **GATK** | [cite_start]MarkDuplicates, BQSR, and Mutect2 [cite: 109, 294] |
| **SnpEff / SnpSift** | [cite_start]Functional annotation and variant filtering [cite: 116, 536] |

---

## 6. Citation

If using this pipeline or analysis, please cite:
> [cite_start]Adil Sukumar, *Somatic Variant Analysis of CD56+ Circulating Tumour Cells in Small Cell Lung Cancer (ERR6473446)*, Sequensolutions Internship Project, 2026[cite: 1, 643, 648, 652].

---

## 7. License

[cite_start]This project is conducted under the **Sequensolutions** bioinformatics framework[cite: 637, 682].
