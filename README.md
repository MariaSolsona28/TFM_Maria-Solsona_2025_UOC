# TFM_Maria-Solsona_2025_UOC

A step-by-step pipeline for analyzing RNA-sequencing data for my Final Master's Project (TFM) as part of the Joint University Master's Degree in Bioinformatics and Biostatistics (UOC, UB).

Email: msolsonag@uoc.edu


**Table of contents**

**Run using command line tools (bash):**

1. Data processing
2. Quality control of pre-aligned reads (FastQC and MultiQC)
3. Adapter Trimming (Fastp)
4. Quality control of trimmed and pre-aligned reads (FastQC and MultiQC post trimming)
5. Alignment with reference genome (HISAT2, SAMtools)
   - Reference genome (SRR27943849.fasta and indexed SRR27943849.fasta.fai)
   - Trimmed reads
6. FeatureCounts
   - Reference genome (SRR27943849.fasta) and GTF file from reference (SA_678_BaktaCLEAN.gtf)
   - Counts list 


**Run in R:**




**Code**

**1. Data processing**

**2. Quality control of pre-aligned reads**
- With FastQC and MultiQC

**3. Adapter Trimming**
- With Fastp

**4. Quality control of trimmed and pre-aligned reads**
- With FastQC and MultiQC post trimming

**5. Alignment with reference genome**
- With HISAT2, SAMtools
  

**6. FeatureCounts**

- With FeatureCounts 

module load subread

Control samples
- featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM13.txt -p -t gene sorted_aligned_reads_MM13.bam
- featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM14.txt -p -t gene sorted_aligned_reads_MM14.bam
- featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM15.txt -p -t gene sorted_aligned_reads_MM15.bam

CHX samples
- featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM16.txt -p -t gene sorted_aligned_reads_MM16.bam
- featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM17.txt -p -t gene sorted_aligned_reads_MM17.bam
- featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM18.txt -p -t gene sorted_aligned_reads_MM18.bam

OCT samples
- featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM19.txt -p -t gene sorted_aligned_reads_MM19.bam
- featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM20.txt -p -t gene sorted_aligned_reads_MM20.bam
- featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM21.txt -p -t gene sorted_aligned_reads_MM21.bam
