# Arabidopsis_ATACseq_pipeline

A general, easy-to-use ATAC-seq data processing pipeline for quality control, mapping, deduplication, Tn5 shift correction, peak calling, and visualization.  

This pipeline is suitable for **paired-end ATAC-seq data** and can be adapted for various organisms.

---

## Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)

---

## Requirements

The following tools should be installed and available in your `$PATH`:

| Tool       | Purpose                                |
|-----------|----------------------------------------|
| fastp     | QC & adapter trimming                   |
| bowtie2   | Mapping reads to reference genome       |
| samtools  | BAM file manipulation (sort, index)     |
| Picard    | Remove PCR duplicates                   |
| deepTools | Tn5 shift correction & bigwig creation |
| MACS2     | Peak calling                            |

Optional: `conda` environment recommended for easy installation.

---

## Installation

```bash
# Create conda environment (optional)
conda create -n atac_seq_env python=3.10
conda activate atac_seq_env

# Install tools
conda install -c bioconda fastp bowtie2 samtools picard macs2 deeptools
