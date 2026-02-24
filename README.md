# 🧬 Somatic Variant Analysis of CD56+ CTCs in SCLC

This repository contains the complete bioinformatics analysis pipeline—from raw sequencing data to a prioritized somatic variant list—for the liquid biopsy sample **ERR6473446**.

The project demonstrates an end-to-end workflow to identify significant genetic variants in **CD56+ Circulating Tumor Cells (CTCs)** from Small Cell Lung Cancer (SCLC) patients. This approach offers a "liquid window" into tumor heterogeneity and evolution that traditional solid biopsies often miss.

---

## 1. Project Overview

### 🔬 About the Data
Raw Whole Exome Sequencing (WES) data was obtained from the European Nucleotide Archive (ENA).

| Field | Value |
| :--- | :--- |
| **Sample Accession** | ERR6473446 |
| **Marker Target** | CD56 (NCAM1) |
| **Cancer Type** | Small Cell Lung Cancer (SCLC) |
| **Strategy** | Whole Exome Sequencing (WES) |
| **Reference Study** | Ricordel et al., *Scientific Reports* (2023) |
| **Reference Genome** | GRCh38 (hg38) |

**Study Background:**
SCLC is a highly aggressive cancer characterized by rapid doubling time and early metastasis. Because traditional tissue biopsies are often necrotic or limited, this project focuses on **CD56+ Liquid Biopsy** to capture the full genetic diversity of the tumor landscape.

---

## 2. Methodology & Pipeline

The analysis was performed in a **Linux (Ubuntu)** environment utilizing **AWS EC2** and **Docker** for containerized tool management.

### Step 1 — Data Acquisition & QC
* Downloaded raw reads using `prefetch` and `fasterq-dump` from the SRA Toolkit.
* Assessed read fidelity using **FastQC** and **MultiQC**, verifying a **Phred score > 28**.

### Step 2 — Alignment & Post-Processing
* Mapped reads to **GRCh38** using **BWA-MEM** with 16 threads.
* Converted SAM to **BAM**, followed by coordinate sorting and indexing via **Samtools**.
* Used **GATK AddOrReplaceReadGroups** and **MarkDuplicates** to identify and flag PCR duplicates.
* Performed **Base Quality Score Recalibration (BQSR)** to correct systematic errors in base quality.

### Step 3 — Somatic Variant Calling
* Identified somatic mutations using **GATK Mutect2** in Tumor-Normal mode.
* Utilized the **gnomAD** resource to filter out common population germline variants.

### Step 4 — Functional Annotation & Filtering
* Predicted functional consequences using **SnpEff** (GRCh38.99 database).
* Filtered for **HIGH** and **MODERATE** impact variants using **SnpSift**.
* Specifically isolated variants altering the protein-coding sequence (e.g., stop-gained, frameshifts).

---

## 3. Key Analytical Findings

The analysis confirmed a highly mutated and heterogeneous tumor profile, capturing the "sum" of metastases.

### 📊 Variant Summary (Key Drivers)
| Impact Tier | Dominance (%) | Key Genes Affected |
| :--- | :--- | :--- |
| **Indel/Frameshift** | 85.7% | TP53, CREBBP, EP300, COL22A1, PTEN, NOTCH3 |
| **Non-Sense** | 14.3% | RB1 |

---

## 4. Key Driver Mutations & Biological Impact

| Gene | Variant Type | Biological Impact & Mechanism |
| :--- | :--- | :--- |
| **RB1** | Non-Sense | Abolishes cell-cycle brakes, triggering uncontrolled proliferation. |
| **TP53** | Frameshift | Nullifies p53 protein, preventing DNA repair and enabling tumor survival. |
| **CREBBP** | Frameshift | Disrupts histone acetylation, causing silencing of tumor-suppressor genes. |
| **EP300** | Frameshift | Aborts protein synthesis, disabling chromatin remodeling. |
| **PTEN** | Frameshift | Induces signaling alterations in the PI3K pathway. |
| **COL22A1** | Frameshift | Destabilizes ECM, impairing tissue integrity to facilitate metastasis. |
| **NOTCH3** | Frameshift | Disrupts neuroendocrine differentiation, driving SCLC progression. |

---

## 5. Interpretation & Clinical Context

### Tumor Heterogeneity & Load
Comparative analysis showed that CD56+ CTCs consistently capture a massive mutation load, **20 to 30 times higher** than matched tissue biopsies. This confirms that liquid biopsy provides a more comprehensive view of the evolving SCLC landscape.

### Mutational Signature Shift
* **Tissue Biopsy:** Massive spike in **C>A transversions**, representing the "smoking gun" of tobacco damage.
* **CD56+ CTCs:** Significant shift toward **C>T transitions**, indicating that circulating cells acquire new evolutionary mutations unrelated to the original smoking damage.

---

## 6. Tools & Dependencies

| Tool | Purpose |
| :--- | :--- |
| **SRA Toolkit** | Raw data acquisition (`prefetch`, `fasterq-dump`) |
| **FastQC / MultiQC** | Quality control and reporting |
| **BWA-MEM** | Read alignment to GRCh38 |
| **Samtools** | BAM conversion, sorting, and indexing |
| **GATK** | MarkDuplicates, BQSR, and Mutect2 |
| **SnpEff / SnpSift** | Functional annotation and variant filtering |

---

## 7. Citation

If using this pipeline or analysis, please cite:
> Adil Sukumar, *Somatic Variant Analysis of CD56+ Circulating Tumour Cells in Small Cell Lung Cancer (ERR6473446)*, Sequensolutions Internship Project, 2026.

---

## 8. License

This project is conducted under the **Sequensolutions** bioinformatics framework.
