# TFM_Maria-Solsona_2025_UOC

A step-by-step pipeline for analyzing RNA-sequencing data for my Final Master's Project (TFM) as part of the Joint University Master's Degree in Bioinformatics and Biostatistics (UOC, UB).

Email: msolsonag@uoc.edu


# **Table of contents**

**Part I: Run using command line tools (bash):**

1. Data processing
2. Quality control of pre-aligned reads (FastQC and MultiQC)
3. Adapter Trimming (Fastp)
4. Quality control of trimmed and pre-aligned reads (FastQC and MultiQC post trimming)
5. Alignment and mapping reads to reference genome (HISAT2, SAMtools)
   - Reference genome (SRR27943849.fasta and indexed SRR27943849.fasta.fai)
   - Trimmed reads
6. Read summarisation or counting mapped reads (FeatureCounts)
   - Reference genome (SRR27943849.fasta) and GTF file from reference (SA_678_BaktaCLEAN.gtf)
   - Counts list table for each indiviudal sample



**Part II: Run in RStudio:**

7. Counts list table (Merge all the samples in one file to import in RStudio)
8. Import Metadata
9. Quality control before normalization
10. DESeq2 analysis (Normalization and identification of DEGs)
11. Studying DEGs (e.g. PCA plot, MA plot, Dispersion plot, Volcano plot, Heatmaps)
12. Venn diagram and Pathway analysis (UniProt)



# **Script**

## Part I. Bash

## 1. Data processing

Make your directory using `mkdir`, and upload all the RNA-Sequencing raw reads (pair-end reads in fastq.gz format) and the reference genome (in fasta format and GTF) 

1.1) Pair-end RNA-Seq reads from the different conditions
- Control (MM13, MM14, MM15)
- CHX (MM16, MM17, MM18)
- OCT (MM19, MM20, MM21)

1.2) Reference genome
- FASTA: SRR27943849.Fasta.gz
- GTF file annotated from reference: SA_678BaktaCLEAN.gtf 


```bash
mkdir Maria_Solsona_data/Staph RNA seq/output_folder
```

## 2. Quality control (FastQC and MultiQC)

## FastQC
Use`fastqc`, to assess sequence quality, running the following commands:

```bash
module load fastqc

- Control samples
fastqc MM13_R1_001.fastq.gz --extract -o /home/output_folder/gzip
fastqc MM13_R2_001.fastq.gz --extract -o /home/output_folder/gzip

fastqc MM14_R1_001.fastq.gz --extract -o /home/output_folder/gzip
fastqc MM14_R2_001.fastq.gz --extract -o /home/output_folder/gzip

fastqc MM15_R1_001.fastq.gz --extract -o /home/output_folder/gzip
fastqc MM15_R2_001.fastq.gz --extract -o /home/output_folder/gzip


- CHX samples
fastqc MM16_R1_001.fastq.gz --extract -o /home/output_folder/gzip
fastqc MM16_R2_001.fastq.gz --extract -o /home/output_folder/gzip

fastqc MM17_R1_001.fastq.gz --extract -o /home/output_folder/gzip
fastqc MM17_R2_001.fastq.gz --extract -o /home/output_folder/gzip

fastqc MM18_R1_001.fastq.gz --extract -o /home/output_folder/gzip
fastqc MM18_R2_001.fastq.gz --extract -o /home/output_folder/gzip


- OCT samples
fastqc MM19_R1_001.fastq.gz --extract -o /home/output_folder/gzip
fastqc MM19_R2_001.fastq.gz --extract -o /home/output_folder/gzip

fastqc MM20_R1_001.fastq.gz --extract -o /home/output_folder/gzip
fastqc MM20_R2_001.fastq.gz --extract -o /home/output_folder/gzip

fastqc MM21_R1_001.fastq.gz --extract -o /home/output_folder/gzip
fastqc MM21_R2_001.fastq.gz --extract -o /home/output_folder/gzip
```



## MultiQC

Use`multiqc` to consolidate all the outputs in a single HTML report, running the following commands:

```bash
module load multiqc

multiqc .
```

## 3. Adapter trimming (Fastp)
Use`fastp`, to perform adapter trimming from the Illumina pair-end reads:


```bash
module load fastp

- For example, make a new folder (fastqc_trim_output_folder) and for sample MM13 (Read 1 and Read 2):

fastp -i /Maria_Solsona_data/Staph_RNA_seq/MM13_R1_001.fastq.gz -I /Maria_Solsona_data/Staph_RNA_seq/MM13_R2_001.fastq.gz --out1 MM13_output_R1.fastq.gz --out2 MM13_output_R2.fastq.gz --trim_poly_g --trim_poly_x --cut_right --cut_window_size 4 --cut_mean_quality 20 --detect_adapter_for_pe -h report.html -j report.json

- Repeat for all the samples:
for i in {13..21}; do fastqc -o fastqc_trim_output_folder MM${i}_output_R1.fastq.gz MM${i}_output_R2.fastq.gz
```



## 4.  Quality control of trimmed and pre-aligned reads (FastQC and MultiQC post trimming)

Use the same commands `fastqc` and `multiqc` as explained before (Section 2.)


## 5. Alignment and mapping reads to reference genome

Use`hisat2`, to aligned the previously trimmed reads with the refrence genome (SRR27943849.fasta)

```bash
module load hisat2
module load samtools
module load flagstat

- Build an index for the reference genome
hisat2-build SRR27943849.fasta genome_index

- Aligned trimmed reads to the reference genome (indexed SRR27943849.fasta.fai)
hisat2 -x genome_index -1 
/Maria_Solsona_data/Staph_RNA_seq/trim_output/MM13_outpu
 t_R1.fastq.gz -2 
/Maria_Solsona_data/Staph_RNA_seq/trim_output/MM13_outpu
 t_R2.fastq.gz -S aligned_reads.sam

- Check SAM output
head aligned_reads.sam

- Convert SAM to BAM format with SAMtools
samtools view -bS aligned_reads.sam > aligned_reads.bam

- Sort and Index BAM file
samtools sort aligned_reads.bam -o sorted_aligned_reads.bam
samtools index sorted_aligned_reads.bam

- Check BAM output
samtools quickeck sorted_Aligned_reads.bam
view -H sorted_aligned_reads.bam

- Summary of the alignment with flagstat
samtools flagstat sorted_aligned_reads.bam
```


## 6. FeatureCounts

Use`featureCounts` for read summarization running the following commands:

```bash
module load subread

- Control samples
featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM13.txt -p -t gene sorted_aligned_reads_MM13.bam
featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM14.txt -p -t gene sorted_aligned_reads_MM14.bam
featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM15.txt -p -t gene sorted_aligned_reads_MM15.bam

- CHX samples
featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM16.txt -p -t gene sorted_aligned_reads_MM16.bam
featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM17.txt -p -t gene sorted_aligned_reads_MM17.bam
featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM18.txt -p -t gene sorted_aligned_reads_MM18.bam

- OCT samples
featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM19.txt -p -t gene sorted_aligned_reads_MM19.bam
featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM20.txt -p -t gene sorted_aligned_reads_MM20.bam
featureCounts -a SA_678BaktaCLEAN.gtf -o output_file_MM21.txt -p -t gene sorted_aligned_reads_MM21.bam

```

Output files are tab-delimited text files for each sorted_aligned reads_samples. A merged file (Output_All), which consolidates the information from all the samples in one table, is created and imported to RStudio for further analysis (Part II) 


## Part II. RStudio
Code and results from RStudio are saved in RMarkdown and HTML format in this GitHub. 
