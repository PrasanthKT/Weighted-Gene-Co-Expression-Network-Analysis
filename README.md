## Transcriptomic Analysis of SARS-CoV-2-Infected Human Respiratory Cells

### Objective 
The primary goal of this project is to investigate transcriptomic changes in human respiratory cells induced by SARS-CoV-2 infection. The analysis aims to identify differentially expressed genes (DEGs) across two conditions: infected versus control samples and across two time points (24 hours and 72 hours post-infection). Comprehensive gene ontology (GO) enrichment and pathway analyses are performed to uncover the biological processes, molecular functions, and cellular components impacted by the viral infection.

### Data Source
Data for this project is available through the National Center for Biotechnology Information (NCBI) under the accession BioProjectID: PRJNA901149.

### Tools/Dependences
1. Conda
2. Bioconda
3. SRA-Toolkit
4. Fastqc
5. Cutadapt
6. Hisat2
7. Sam-tools
8. Subread
9. DESeq2 (R)

### Installation
1. Create a conda environment for the project
```bash
conda create -n rna_env
conda activate rna_env
```
2. Install the tools using the following command in the created environment.
```bash
conda install -c bioconda sra-tools fastqc cutadapt hisat2 samtools subread
```
3. For DESeq2 in R
```R
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("DESeq2")
```
4. GO Annotation packages in R
```R
BiocManager::install("clusterProfiler")
BiocManager::install("org.Hs.eg.db"
```
### Reference Genome and Annotation file
1. Download the Human reference genome file (vGRCh38).
```bash
wget ftp://ftp.ensembl.org/pub/release-109/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
gunzip Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
```
2. Download the Human gene annotation file (vGRCh38). 
```bash
wget ftp://ftp.ensembl.org/pub/release-109/gtf/homo_sapiens/Homo_sapiens.GRCh38.109.gtf.gz
gunzip Homo_sapiens.GRCh38.109.gtf.gz 
``` 
### Download the data using the SRA Toolkit
```bash
prefetch SRP407642
```
### Analysis
The scripts required for each analysis step can be found in the ```Scripts``` folder in the repository. Before running these scripts, update any directory paths, environment names, and SLURM account details to match your system configuration.

```fastq-dump.sh``` - Convert the .sra files to fastq format.

```fastqc.sh``` - Performs Quality Control checks on the .fastq files.

```cutadapt.sh``` - Trims adapter sequences from reads.

```mapping.sh``` - Maps the reads to the refernce genome using HISAT2 for alignment.

```sam_to_bam.sh``` - Converts .sam files to .bam format for efficient storage.

```featureCounts.sh``` - Quantifies gene expression by counting features in .bam files using the .gtf annotation file with Subread and the output is saved to .csv format.

### Differential gene expression analysis.
A Detailed R script ```DESeq2.R``` for performing differential gene expression analysis is provided in the repository. This script utilizes DESeq2 to identify differentially expressed genes between conditions. The initial feature_counts.csv file is preprocessed using pandas in Python, producing gene_counts_only.csv as the input for the DESeq2. The raw files, processed file and Python script used are present in ```DESeq2``` directory in the repository. 

### Output files
The output files generated at each step serve as inputs for subsequent analyses. The final results, along with a comprehensive summary of the workflow and findings, are compiled in the ```Report.pdf``` document located in the repository.

