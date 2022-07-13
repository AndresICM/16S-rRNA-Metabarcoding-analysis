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
Sixteen files are part of this tutorial. BA stands for Bioaugmentation and LF to landfarming, which was the control treatment. Then Txx indicate the time of sampling. T01 is the initial point, T03, T07 and T11 are at the 2nd, 6th and 10th week respectively. The after the - appears a R1 or R2, which correspond to the forward and reverse reads of a sample

## Assemble Forward and Reverse Reads

Now, let's get into mothur, just typing mothur on your terminal. Then we can use the make.file command to make a file which indicates the forward and reverse sequence of a sample. This file is used latter to assemble both sequences
```
mothur

make.file(inputdir=Demultiplexed/, type=fastq, prefix=tutorial)
```
In this command, in the inputdir, we selected the folder with the raw files; type indicates the file extension; and the prefix is the name that we want assign to all of our subsequent files.

Now, let's inspect the output file in a different terminal window, so we don't have to quit Mothur yet. 

```
less Demultiplexed/tutorial.files

Group_0 BA3T01-R1.fastq BA3T01-R2.fastq 
Group_1 BA3T03-R1.fastq BA3T03-R2.fastq 
Group_2 BA3T07-R1.fastq BA3T07-R2.fastq 
Group_3 BA3T11-R1.fastq BA3T11-R2.fastq 
Group_4 LF3T01-R1.fastq LF3T01-R2.fastq 
Group_5 LF3T03-R1.fastq LF3T03-R2.fastq 
Group_6 LF3T07-R1.fastq LF3T07-R2.fastq 
Group_7 LF3T11-R1.fastq LF3T11-R2.fastq 
```
So, Mothur created a file with the eight samples and the raw read associated to each. Then, if you just press 'q', you'll quit the less command.
Now we can use that file to assemble the reads.

```
make.contigs(file=Demultiplexed/tutorial.files, processors=4, inputdir=Demultiplexed, outputdir=Contigs)

Group count: 
Group_0	64716
Group_1	86371
Group_2	68128
Group_3	83251
Group_4	68419
Group_5	74009
Group_6	83255
Group_7	94209

Total of all groups is 622358

It took 188 secs to process 622358 sequences.

Output File Names: 
Contigs/tutorial.trim.contigs.fasta
Contigs/tutorial.scrap.contigs.fasta
Contigs/tutorial.contigs_report
Contigs/tutorial.contigs.count_table
```
This command assembles the forward and reverse sequences and generate an individual fasta file with quality indexes in it. Additionally, generates a count-table and a report file which gives information of the group identitity, and information about the assembly of each read respectively. 

We can inspect our data using summary.seqs

```
summary.seqs(fasta=Contigs/tutorial.trim.contigs.fasta)

Using 4 processors.

		Start	End	NBases	Ambigs	Polymer	NumSeqs
Minimum:	1	247	247	0	3	1
2.5%-tile:	1	253	253	0	4	15559
25%-tile:	1	253	253	0	4	155590
Median: 	1	253	253	0	4	311180
75%-tile:	1	253	253	0	5	466769
97.5%-tile:	1	274	274	11	6	606800
Maximum:	1	502	502	68	251	622358
Mean:	1	254	254	1	4
# of Seqs:	622358

It took 6 secs to summarize 622358 sequences.

Output File Names:
Contigs/tutorial.trim.contigs.summary
```
This tells us that we have 622358  sequences. Most of them are between 247 and 274 bp. Approximately 2.5% of our sequences have some ambiguous bases. Since we sequenced the V4 region of the rRNA 16S, it’s quite unusual to see the same nuvleotide repeted more than 8 times. For that matter, to delete undesired sequences we will use the screen.seqs command.


```
```

```
```

```
```




## Reduce Sequencing Errors

```
```

```
```

```
```

```
```
```
```

```
```

```
```

```
```

