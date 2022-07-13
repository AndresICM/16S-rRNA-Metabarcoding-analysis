# 16S-rRNA-Metabarcoding-Analysis
Tutorial for the analysis of 16S rRNA metabarcoding sequences using Illumina pairend reads. This tutorial is based on the MiSeq SOP pipeline (https://mothur.org/wiki/miseq_sop/ Schloss et al., 2013.)

## Introduction

Sequences were generated using data from a hydrocarbon bioremediation project. Two treatments were selected for this tutorial, a bioaugmentation with *Acinetobacter, Pseudomonas* and *Rhodococcus* strains, and a control. 
Both treatments were inoculated with a high concentration of diesel before the beggining of the experiment, and were periodically turned over for aereation. Temperature, pH, total pretroleum hydrocarbons (TPH) and other physico-chemical parameters were monitored. 
The fundamental question of the experiment was observe the bacterial communities changes accross the experiment, and evaluate how they change while the TPH concentration decreases.

# Getting Started

## Download Raw Data

The data is publicly available in Zenodo. To download it we will create first a folder called 16S_tutorial and enter into that folder with cd

```
mkdir 16S_tutorial
cd 16S_tutorial
```

Then we will download the data using wget

```
wget https://zenodo.org/record/6828213/files/Demultiplexed.tar.xz
```
Finally we will extract the downloaded file

```
tar -xf Demultiplexed.tar.xz
```
A folder called Demultiplexed should be created. Let's inspect what's inside

```
ls Demultiplexed

BA3T01-R1.fastq  BA3T07-R1.fastq  LF2T11-R1.fastq  LF3T03-R1.fastq
BA3T01-R2.fastq  BA3T07-R2.fastq  LF2T11-R2.fastq  LF3T03-R2.fastq
BA3T03-R1.fastq  BA3T11-R1.fastq  LF3T01-R1.fastq  LF3T07-R1.fastq
BA3T03-R2.fastq  BA3T11-R2.fastq  LF3T01-R2.fastq  LF3T07-R2.fastq
```
Sixteen files are part of this tutorial. BA stands for Bioaugmentation and LF to landfarming, which was the control treatment. Then Txx indicate the time of sampling. T01 is the initial point, T03, T07 and T11 are at the 2nd, 6th and 10th week respectively. The after the - appears a R1 or R2, which correspond to the forward and reverse reads of a sample. 


