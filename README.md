# 🧬 WES Variant Analysis — Triple Negative Breast Cancer (TNBC)

This repository contains the complete bioinformatics analysis pipeline — from raw sequencing data to a prioritized variant list — for Whole Exome Sequencing (WES) sample `SRR34853843`.

The project demonstrates an end-to-end workflow to identify significant genetic variants in a Triple Negative Breast Cancer (TNBC) sample from a **South Asian cohort**, a population historically underrepresented in large-scale genomic studies.

---

## 1. Project Overview

### 🔬 About the Data

Raw sequencing data was obtained from the NCBI Sequence Read Archive (SRA).

| Field | Value |
|---|---|
| **Study** | PRJNA1298555 |
| **BioSample** | SAMN50279194 |
| **Run ID** | SRR34853843 |
| **Strategy** | Whole Exome Sequencing (WXS) |
| **Instrument** | Illumina NovaSeq X |
| **Reference Genome** | GRCh38 (hg38) |

**Study Background:**
TNBC is a genetically heterogeneous malignancy with diverse mutational profiles across different ethnic backgrounds. This study addresses a significant gap in knowledge for South Asian populations by identifying clinically actionable genetic variants for potential therapeutic intervention.

---

## 2. Methodology & Pipeline

A standard bioinformatics pipeline was applied to process raw FASTQ reads, call variants, and annotate their functional consequences.

### Step 1 — Quality Control
- Raw paired-end reads (`.fastq.gz`) were assessed using **FastQC** for base quality scores, adapter content, GC distribution, and duplication levels.

### Step 2 — Alignment & Post-Processing
- Reads were aligned to **GRCh38** using a short-read aligner.
- The resulting BAM file was sorted and indexed with **Samtools**.
- PCR duplicates were identified and marked using **Picard MarkDuplicates**.

### Step 3 — Variant Calling
- Variants (SNPs and Indels) were called from the processed BAM to generate a raw `.vcf` file.

### Step 4 — Functional Annotation
- Variants were annotated with **SnpEff** (GRCh38 database) to predict functional consequences: missense, frameshift, stop-gained, splice-site, start-lost, etc.

### Step 5 — Filtering & Prioritization
- The annotated VCF was filtered by impact tier (HIGH / MODERATE).
- Final results are summarized in `.tsv` output files.

---

## 3. Key Analytical Findings

The pipeline successfully processed and annotated **815,355 total variants**. After filtering for functional significance:

### 📊 Variant Summary

| Impact Tier | Variant Types | Examples |
|---|---|---|
| **HIGH** | Frameshift, stop-gained, splice-acceptor/donor, start-lost | `SETBP1`, `CDH2`, `NCOA3`, `ANKLE1`, `SMARCA4` |
| **MODERATE** | Missense, disruptive in-frame indels | `RBBP8`, `ROCK1`, `LAMA3`, `MUC16`, `APOBEC3B` |

---

### 🔴 High-Impact Variants

High-impact variants are disruptive mutations likely to result in non-functional proteins. Key findings include:

#### Tumor Suppressor & DNA Repair Genes
| Gene | Variant Type | Clinical Relevance |
|---|---|---|
| `SETBP1` | Frameshift | Oncogene implicated in myeloid malignancies; frameshift disrupts SET-binding |
| `RBBP8` (CtIP) | Missense | Encodes CtIP; critical for DNA end-resection in homologous recombination; mutations linked to BRCA-like phenotype |
| `ANKLE1` | Frameshift (×2) | Endonuclease involved in DNA damage response; recently linked to breast cancer susceptibility |
| `CDH2` | Stop-gained | N-Cadherin; loss promotes epithelial-mesenchymal transition (EMT) in TNBC |

#### Chromatin Remodeling
| Gene | Variant Type | Clinical Relevance |
|---|---|---|
| `SMARCA4` | Missense | SWI/SNF complex ATPase; loss is synthetic lethal with SMARCA2 — druggable target |
| `ASXL3` | Missense | Polycomb-associated chromatin regulator; frequent in breast cancers |

#### Cell Signaling & Oncogenesis
| Gene | Variant Type | Clinical Relevance |
|---|---|---|
| `NCOA3` (SRC-3/AIB1) | Frameshift | Steroid receptor coactivator amplified in breast cancer; frameshift is loss-of-function |
| `ROCK1` | Missense | Rho-kinase; promotes cancer cell invasion; validated therapeutic target |
| `JAK3` | Missense | Tyrosine kinase in cytokine signaling; JAK inhibitors are in clinical use |
| `PIK3R2` | Missense | PI3K regulatory subunit; PI3K/AKT pathway is frequently altered in TNBC |
| `AURKA` | Missense | Aurora kinase A; centrosome/mitosis regulator; clinical trials ongoing for TNBC |

#### Structural & Gene Fusion
| Event | Type | Clinical Relevance |
|---|---|---|
| `DERL3–SMARCB1` | Bidirectional gene fusion (HIGH) | SMARCB1 is a canonical tumor suppressor; fusion events involving it are highly actionable |

#### Notable Additional HIGH-Impact Genes
- `LAMA3` — Laminin subunit; basement membrane integrity, invasion suppression
- `SERPINB11` — Serine protease inhibitor (stop-gained)
- `NPHP4` — Splice acceptor variant
- `CAMTA1` — Start lost
- `PER3` — Circadian rhythm gene; frameshift
- `KIF1B` — Neuronal kinesin; frameshift
- `TARDBP` — RNA-binding protein; stop-gained
- `ALK` — Anaplastic lymphoma kinase; stop-gained (druggable)
- `FCGBP` — Frameshift in mucin-binding protein

---

### 🟡 Moderate-Impact (Missense) Variants

A large number of missense variants were identified. Biologically and clinically notable findings include:

#### DNA Damage & Repair
| Gene | Variant | Notes |
|---|---|---|
| `RBBP8` | Missense | CtIP; BRCA1-interactor — potential BRCAness marker |
| `MTHFR` | p.Ala222Val | Very common folate-metabolism polymorphism; may affect chemotherapy response |
| `OGG1` | Missense | Base excision repair enzyme; oxidative DNA damage pathway |
| `MLH1` | Missense (×2) | Mismatch repair gene; Lynch syndrome gene — potential microsatellite instability indicator |

#### Immune & Tumor Microenvironment
| Gene | Notes |
|---|---|
| `APOBEC3B` / `APOBEC3F` / `APOBEC3H` | Multiple missense variants; APOBEC cytidine deaminases are a major mutational signature (SBS2/SBS13) in TNBC |
| `HLA-A` / `HLA-DPB1` | Multiple missense variants; expected HLA heterozygosity; impacts neoantigen presentation |
| `IL12RB1` | Missense + splice-acceptor (HIGH); IL-12 receptor subunit critical for anti-tumor immunity |
| `JAK3` | Missense; JAK-STAT pathway; immunotherapy relevance |
| `CD22`, `ADGRE1`, `ADGRE3`, `ADGRE5` | Immune cell surface markers |

#### Invasion, Adhesion & EMT
| Gene | Notes |
|---|---|
| `CDH2`, `CDH4`, `CDH7`, `CDH19`, `CDH22`, `CDH26` | Cadherin family; multiple missense variants suggest broad cell adhesion remodeling |
| `LAMA3`, `LAMA5` | Laminin subunits; extracellular matrix components involved in invasion |
| `COL5A3`, `COL6A1`, `COL7A1`, `COL9A1`, `COL12A1`, `COL18A1`, `COL19A1` | Collagen family; extensive ECM remodeling signature |
| `ROCK1` | Rho-kinase; invasion and metastasis |

#### Cancer Antigen & Mucin
| Gene | Notes |
|---|---|
| `MUC16` | Numerous missense variants (>100 positions); encodes CA-125 cancer antigen; hypermutation may reflect ongoing immune evasion |
| `FCGBP` | Multiple missense + frameshift; mucin-binding protein |

#### Cell Cycle & Proliferation
| Gene | Notes |
|---|---|
| `AURKA` | Missense; Aurora kinase A — centrosome amplification, mitotic entry |
| `CCNA2` | Missense; Cyclin A2; cell cycle control |
| `SETD2` | Missense; H3K36 methyltransferase, frequently mutated in cancers |
| `SETBP1` | Missense + frameshift; SET-binding oncogene |

#### Other Clinically Relevant Genes
| Gene | Notes |
|---|---|
| `SMARCA4` | Missense; SWI/SNF; synthetic lethality target |
| `EP300` | Missense; Histone acetyltransferase; transcriptional coactivator |
| `TMPRSS2` | Missense; serine protease; role in cancer invasion |
| `DCC` | Missense (×2); Deleted in Colorectal Cancer; tumor suppressor |
| `SIRT6` | Missense; deacetylase; DNA repair and metabolic regulation |
| `GDF15` | Missense; TGF-β superfamily; immune evasion in cancer |
| `NOTCH3` | Missense; Notch pathway; stem cell maintenance in TNBC |

---

## 4. Interpretation & Clinical Context

### APOBEC Mutational Signature
The high density of `APOBEC3B/3F/3H` missense variants, combined with C→T and C→G mutations at TC dinucleotide contexts, is consistent with **APOBEC-driven mutagenesis** (SBS2 and SBS13). This signature is prevalent in TNBC and has implications for immune checkpoint responsiveness.

### BRCAness Phenotype
Mutations in `RBBP8` (CtIP/CtBP-interacting protein), `ANKLE1`, and the `DERL3–SMARCB1` fusion event collectively suggest a potential **BRCAness** phenotype — homologous recombination deficiency — even in the absence of a canonical BRCA1/2 mutation. This may predict sensitivity to **PARP inhibitors** or **platinum-based chemotherapy**.

### PI3K/JAK Pathway Disruption
Concurrent variants in `PIK3R2` and `JAK3` suggest dysregulation of both the **PI3K/AKT/mTOR** and **JAK/STAT** axes — pathways with approved targeted therapies relevant to TNBC (e.g., alpelisib, ruxolitinib-class agents).

### South Asian Population Context
This sample fills a critical gap in TNBC genomic databases, which are dominated by European-ancestry cohorts. Variants identified here, particularly in `MTHFR`, `APOL1/4`, and immune-related loci (`HLA`, `IL12RB1`, `ADGRE` family), may have population-specific frequency profiles warranting comparison against gnomAD South Asian allele frequencies.

---

## 5. Repository Contents
.
├── fastqc/
│ ├── SRR34853843_1_fastqc.html # FastQC report — Read 1
│ └── SRR34853843_2_fastqc.html # FastQC report — Read 2
│
├── metrics/
│ └── metrics.txt # Picard MarkDuplicates output
│
├── annotation/
│ ├── snpEff_summary.html # SnpEff global summary report
│ └── snpEff_genes.txt # Per-gene variant effect breakdown
│
└── results/
├── high_moderate_variants.tsv # All HIGH + MODERATE impact variants
├── filtered_main_variants.tsv # Curated filtered variant list
└── main_findings.tsv # Summary table of key findings

text

---

## 6. Tools & Dependencies

| Tool | Purpose |
|---|---|
| FastQC | Raw read quality assessment |
| Samtools | BAM sorting, indexing, flagstat |
| Picard MarkDuplicates | PCR duplicate flagging |
| Variant Caller (e.g., GATK HaplotypeCaller) | SNP/Indel calling |
| SnpEff | Functional variant annotation (GRCh38) |

---

## 7. Future Directions

- [ ] Compare variant allele frequencies against **gnomAD South Asian** population to distinguish somatic vs. germline candidates
- [ ] Perform **APOBEC mutational signature deconvolution** (SigProfiler or MutationalPatterns)
- [ ] Cross-reference HIGH-impact variants against **ClinVar**, **COSMIC**, and **OncoKB**
- [ ] Validate `DERL3–SMARCB1` fusion using RNA-seq or orthogonal sequencing
- [ ] Assess **HRD (Homologous Recombination Deficiency) score** based on LOH + TAI + LST metrics
- [ ] Submit novel population-specific variants to a public database (e.g., **SeqRxiv** / dbSNP)

---

## 8. Citation

If using this analysis or the pipeline, please cite:
> [Abhishek S R / Sequensolutions], *WES Variant Analysis of a South Asian TNBC Sample (SRR34853843)*, GitHub, 2026.

---

## 9. License

This project is licensed under the **MIT License**. See `LICENSE` for details.
