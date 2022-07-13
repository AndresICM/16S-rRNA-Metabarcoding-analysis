# 16S-rRNA-Metabarcoding-Analysis
Tutorial for the analysis of 16S rRNA metabarcoding sequences using Illumina pairend reads. This tutorial is based on the MiSeq SOP pipeline (https://mothur.org/wiki/miseq_sop/ Schloss et al., 2013.)

## Introduction

Sequences were generated using data from a hydrocarbon bioremediation project. Two treatments were selected for this tutorial, a bioaugmentation with *Acinetobacter, Pseudomonas* and *Rhodococcus* strains, and a control. 
Both treatments were inoculated with a high concentration of diesel before the beggining of the experiment, and were periodically turned over for aereation. Temperature, pH, total pretroleum hydrocarbons (TPH) and other physico-chemical parameters were monitored. 
The fundamental question of the experiment was observe the bacterial communities changes accross the experiment, and evaluate how they change while the TPH concentration decreases.

##Getting Started

##Download Raw Data

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



