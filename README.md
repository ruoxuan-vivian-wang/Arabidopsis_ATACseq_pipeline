# Arabidopsis_ATACseq_pipeline

A general, easy-to-use ATAC-seq data processing pipeline for quality control, mapping, deduplication, Tn5 shift correction, peak calling, and visualization.  

This pipeline is suitable for **paired-end ATAC-seq data** and can be adapted for various organisms.

---

## Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [Pipeline Overview](#pipeline-overview)
- [Usage](#usage)
- [Outputs](#outputs)
- [Notes](#notes)
- [References](#references)

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
````

---

## Pipeline Overview

Steps included in this pipeline:

1. **QC & Adapter Trimming** (`fastp`)
   Remove adapters, low-quality bases, and generate HTML/JSON QC reports.

2. **Mapping to Reference Genome** (`bowtie2`)
   Align reads to the genome and convert to BAM format.

3. **Sort & Add Read Groups** (`samtools`)
   Sort BAM files and add sample-specific read group info.

4. **Remove PCR Duplicates** (`Picard`)
   Eliminate PCR amplification bias.

5. **Tn5 Shift Correction** (`alignmentSieve` from deepTools)
   Correct ATAC-seq insertion offset (+4/-5bp) for accurate peak calling.

6. **Peak Calling** (`MACS2`)
   Identify open chromatin regions.

7. **BigWig Track Generation** (`bamCoverage` from deepTools)
   Create normalized signal tracks for visualization in IGV/UCSC.

---

## Usage

1. Clone the repository:

```bash
git clone https://github.com/ruoxuan-vivian-wang/Arabidopsis_ATACseq_pipeline.git
cd Arabidopsis_ATACseq_pipeline
```

2. Edit `Arabidopsis_ATACseq_pipeline.sh`:

```bash
# Set input files, sample name, genome path, genome size, and number of threads
R1=your_sample_R1.fastq.gz
R2=your_sample_R2.fastq.gz
SAMPLE=your_sample
GENOME_FA=/path/to/genome.fa
GENOME_INDEX=/path/to/genome_index
GENOME_SIZE=1.35e8  # Arabidopsis example
THREADS=16
```

3. Run the pipeline:

```bash
bash Arabidopsis_ATACseq_pipeline.sh
```

---

## Outputs

| File/Folder                | Description                                    |
| -------------------------- | ---------------------------------------------- |
| `*_fastp_report.html/json` | QC report after trimming                       |
| `*.bam`                    | Raw aligned BAM                                |
| `*.sorted.bam`             | Sorted BAM                                     |
| `*.rg.bam`                 | BAM with read group information                |
| `*.dedup.bam`              | Deduplicated BAM                               |
| `*.shifted.sorted.bam`     | Tn5 shift-corrected BAM                        |
| `macs2_output/`            | Peak calling results (narrowPeak, summit, etc) |
| `*.bw`                     | BigWig coverage tracks for visualization       |

---

## Notes

* Adjust `GENOME_SIZE` for different organisms.
* Modify `--shift` and `--extsize` in MACS2 for other fragment size distributions.
* Use proper CPU allocation (`THREADS`) based on your system.
* For multiple samples, run this pipeline in parallel or use a loop.

---


