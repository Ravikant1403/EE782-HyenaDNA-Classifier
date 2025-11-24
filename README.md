Modifying HyenaDNA for Classifying Promoter , Enhancer 

🧬 Genomic Motif Classification Using HyenaDNA

This repository contains the complete workflow for preparing a labeled genomic dataset and fine-tuning the **HyenaDNA** long-range genomic model for motif classification.  
The project converts ENCODE cCRE annotations into promoter, enhancer, and junk classes, extracts sequences from the hg38 reference genome, and trains a deep learning classifier.

---

📁 Project Overview

Input Files
- `human.csv` — ENCODE cCRE annotation file containing:
  - Chromosome
  - Start & end coordinates
  - ENCODE IDs
  - 9 functional annotation classes in `comments`
- `hg38.fa` — Human reference genome (GRCh38)

 Output Files
- `human_final.csv` — Cleaned sequences with final labels (promoter, enhancer, junk)
- `train.csv` / `test.csv` — 80/20 split for model training and evaluation
- Model training scripts + configs for HyenaDNA

---

🔬 Motivation

Most regulatory regions in the human genome are non-coding.  
Understanding promoter and enhancer behavior requires high-resolution sequence models capable of modeling long-range dependencies.

This project:
- Converts ENCODE annotations into simplified sequence-level labels
- Extracts exact genomic sequences via BLAST alignment
- Prepares data for training large genomic foundation models (HyenaDNA)

---

🧪 Data Preprocessing Pipeline

The preprocessing pipeline consists of the following stages:

1. Load ENCODE cCRE annotations (human.csv)
2. Map 9 ENCODE classes → 3 final categories
   - Promoter
   - Enhancer
   - Junk
3. Convert genomic coordinates to FASTA queries
4. Run BLAST against hg38 reference
5. Extract nucleotide sequences
6. Remove invalid/ambiguous sequences
7. Generate final dataset: `human_final.csv`
8. Perform 80/20 stratified train–test split

A block diagram summarizing this process is included in the repo.

---

🔗 Class Mapping

| ENCODE Class (Raw)           | Final Category |
|------------------------------|----------------|
| PLS                          | Promoter       |
| TSS-like                     | Promoter       |
| DNase-H3K4me3 promoter-like  | Promoter       |
| pELS                         | Enhancer       |
| dELS                         | Enhancer       |
| Enhancer-H3K4me3             | Enhancer       |
| CTCF-only                    | Junk           |
| CTCF-bound                   | Junk           |
| Unclassified                 | Junk           |

---

🧬 Sequence Extraction via BLAST

Reference genome was indexed using:

```bash
makeblastdb -in hg38.fa -dbtype nucl -out blast_db/hg38_db
