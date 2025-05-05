# TFM_Maria-Solsona_2025_UOC

A step-by-step pipeline for analyzing RNA-sequencing data for my Final Master's Project (TFM) as part of the Joint University Master's Degree in Bioinformatics and Biostatistics (UOC, UB).

Email: msolsonag@uoc.edu


**Table of contents**

**Part I: Run using command line tools (bash):**

1. Data processing
2. Quality control of pre-aligned reads (FastQC and MultiQC)
3. Adapter Trimming (Fastp)
4. Quality control of trimmed and pre-aligned reads (FastQC and MultiQC post trimming)
5. Alignment with reference genome (HISAT2, SAMtools)
   - Reference genome (SRR27943849.fasta and indexed SRR27943849.fasta.fai)
   - Trimmed reads
6. FeatureCounts
   - Reference genome (SRR27943849.fasta) and GTF file from reference (SA_678_BaktaCLEAN.gtf)
   - Counts list for the 3 conditions (Control, CHX, OCT)


**Part II: Run in RStudio:**

7. Counts list (Merge the 3 conditions, import in R)
8. Import Metadata
9. Quality control before normalization
10. DESeq2 analysis (Normalization and identification of DEGs)
11. Understanding of DEGs (PCA plot, Volcano plot, Heatmap)
12. Venn diagram and Pathway analysis (UniProt)






