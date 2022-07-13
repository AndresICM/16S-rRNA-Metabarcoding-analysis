# 16S-rRNA-Metabarcoding-Analysis
Tutorial for the analysis of 16S rRNA metabarcoding sequences using Illumina paired end reads. This tutorial is based on the MiSeq SOP pipeline (https://mothur.org/wiki/miseq_sop/ Schloss et al., 2013.)

## Introduction

Sequences were generated using data from a hydrocarbon bioremediation project. Two treatments were selected for this tutorial, bioaugmentation with *Acinetobacter, Pseudomonas* and *Rhodococcus* strains, and a control. 
Both treatments were inoculated with a high concentration of diesel before the begining of the experiment and were periodically turned over for aeration. Temperature, pH, total petroleum hydrocarbons (TPH) and other physicochemical parameters were monitored. 
The fundamental question of the experiment was to observe the bacterial communities' changes across the experiment, and evaluate how they change while the TPH concentration decreases.

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
Finally, we will extract the downloaded file

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
Sixteen files are part of this tutorial. BA stands for Bioaugmentation and LF for landfarming, which was the control treatment. Then Txx indicates the time of sampling. T01 is the initial point, T03, T07 and T11 are at the 2nd, 6th and 10th week respectively. The after the - appears an R1 or R2, which correspond to the forward and reverse reads of a sample

## Assemble Forward and Reverse Reads

Now, let's get into mothur, just typing mothur on your terminal. Then we can use the make.file command to make a file which indicates the forward and reverse sequence of a sample. This file is used later to assemble both sequences
```
mothur

make.file(inputdir=Demultiplexed/, type=fastq, prefix=tutorial)
```
In this command, the inputdir, we selected the folder with the raw files, the type indicates the file extension, and the prefix is the name that we want to assign to all of our subsequent files.

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
So, Mothur created a file with the eight samples and the raw read associated with each. Then, if you just press 'q', you'll quit the less command.
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
This command assembles the forward and reverse sequences and generates an individual fasta file with quality indexes in it. Additionally, generates a count-table and a report file which gives information of the group identitity and information about the assembly of each read respectively. 

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
This tells us that we have 622358  sequences. Most of them are between 247 and 274 bp. Approximately 2.5% of our sequences have some ambiguous bases. 

## Reduce Sequencing Errors

Since we sequenced the V4 region of the rRNA 16S, it’s quite unusual to see nucleotides repeated more than 8 times. So, to remove undesired reads we will use the screen.seqs command

```
screen.seqs(fasta=Contigs/tutorial.trim.contigs.fasta, count=Contigs/tutorial.contigs.count_table, maxambig=0, maxlength=253, maxhomop=8, inputdir=Contigs/)

Output File Names:
Contigs/tutorial.trim.contigs.good.summary
Contigs/tutorial.trim.contigs.good.fasta
Contigs/tutorial.trim.contigs.bad.accnos
Contigs/tutorial.contigs.good.count_table
```
Now, we can inspect again our sequences (we'll be inspecting quite often our reads)

```
summary.seqs(fasta=Contigs/tutorial.trim.contigs.good.fasta, count=Contigs/tutorial.contigs.good.count_table)


		Start	End	NBases	Ambigs	Polymer	NumSeqs
Minimum:	1	250	250	0	3	1
2.5%-tile:	1	253	253	0	4	11660
25%-tile:	1	253	253	0	4	116599
Median: 	1	253	253	0	4	233198
75%-tile:	1	253	253	0	5	349797
97.5%-tile:	1	253	253	0	6	454736
Maximum:	1	253	253	0	8	466395
Mean:	1	252	252	0	4
# of Seqs:	466395
```
This indicates that now we have 466395 sequences, so, 155963 were removed.

When working when metabarcode sequences, always there are duplicates. Since it’s just too wasteful computationally speaking to align the same sequence every time, we’ll work only with unique files.

```
unique.seqs(fasta=Contigs/tutorial.trim.contigs.good.fasta, count=Contigs/tutorial.contigs.good.count_table)

Output File Names: 
Contigs/tutorial.trim.contigs.good.unique.fasta
Contigs/tutorial.trim.contigs.good.count_table
```

```
summary.seqs(fasta=Contigs/tutorial.trim.contigs.good.unique.fasta, count=Contigs/tutorial.trim.contigs.good.count_table)

		Start	End	NBases	Ambigs	Polymer	NumSeqs
Minimum:	1	250	250	0	3	1
2.5%-tile:	1	253	253	0	4	11660
25%-tile:	1	253	253	0	4	116599
Median: 	1	253	253	0	4	233198
75%-tile:	1	253	253	0	5	349797
97.5%-tile:	1	253	253	0	6	454736
Maximum:	1	253	253	0	8	466395
Mean:	1	252	252	0	4
# of unique seqs:	114887
total # of seqs:	466395
```
So now instead of working with 466395 sequences, we have to work only with 114887

## Make a Customized Database for the 16S rRNA V4 region
To eventually know the taxonomy of your sequences, we have to use a reference database. The SILVA database is the gold standard for it.
We will create a new folder to download the SILVA database and then extract the compressed file

```
mkdir Silva_DB
cd Silva_DB
wget https://mothur.s3.us-east-2.amazonaws.com/wiki/silva.nr_v138_1.tgz
tar -xzvf silva.nr_v138_1.tgz
```
To keep the alignments from position 11890 to 25323 (V4 region), we will go back to the Mothur terminal and use the pcr.seqs command. Also. to delete the trail dots in each alignment, we will set keepdots as false

```
pcr.seqs(fasta=Silva_DB/silva.nr_v138_1.align , start=11890, end=25323, keepdots=F)

Output File Names: 
Contigs/silva.nr_v138_1.pcr.align
```

Let's rename this file

```
rename.file(input=Contigs/silva.nr_v138_1.pcr.align, new=silva.v4.fasta)
```
## Working with Improved Sequences and a Customized Database

Now that we have our improved reads and a customized database, we can align sequences to the reference. This process takes a few minutes

```
align.seqs(fasta=Contigs/tutorial.trim.contigs.good.unique.fasta, reference=Contigs/silva.v4.fasta, processors=1, flip=t)
```
We can inspect the alignment
```
summary.seqs(fasta=Contigs/tutorial.trim.contigs.good.unique.align, count=Contigs/tutorial.trim.contigs.good.count_table, processors=4)

		Start	End	NBases	Ambigs	Polymer	NumSeqs
Minimum:	1	1	1	0	1	1
2.5%-tile:	1973	11555	253	0	4	11660
25%-tile:	1973	11555	253	0	4	116599
Median: 	1973	11555	253	0	4	233198
75%-tile:	1973	11555	253	0	5	349797
97.5%-tile:	1973	11555	253	0	6	454736
Maximum:	13427	13430	253	0	8	466395
Mean:	1973	11554	252	0	4
# of unique seqs:	114887
total # of seqs:	466395
```
We can observe that most of our sequences align between the 1973 and 11555 possition of the reference. Now we can remove the alignments that does not overlap in this region

```
screen.seqs(fasta=Contigs/tutorial.trim.contigs.good.unique.align, count=Contigs/tutorial.trim.contigs.good.count_table, start=1973, end=11555, processors=4)
```

```
summary.seqs(fasta=Contigs/tutorial.trim.contigs.good.unique.good.align, count=Contigs/tutorial.trim.contigs.good.good.count_table)


			Start	End	NBases	Ambigs	Polymer	NumSeqs
Minimum:	1970	11555	250	0	3	1
2.5%-tile:	1973	11555	253	0	4	11646
25%-tile:	1973	11555	253	0	4	116459
Median: 	1973	11555	253	0	4	232917
75%-tile:	1973	11555	253	0	5	349375
97.5%-tile:	1973	11555	253	0	6	454188
Maximum:	1973	13430	253	0	8	465833
Mean:	1972	11555	252	0	4
# of unique seqs:	114514
```
We can see that 562 sequences were removed. Now we can delete some gap characters from our file, to reduce its volume 

```
filter.seqs(fasta=Contigs/tutorial.trim.contigs.good.unique.good.align, vertical=T, trump=.)

Length of filtered alignment: 623
Number of columns removed: 12811
Length of the original alignment: 13434
Number of sequences used to construct filter: 114514
```
So we went from an alignment file with 13434 columns to one of 623. 
Now, we might still have some redundant sequences, therefore we will re run the unique.seqs command

```
unique.seqs(fasta=Contigs/tutorial.trim.contigs.good.unique.good.filter.fasta, count=Contigs/tutorial.trim.contigs.good.good.count_table)
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
```

```
```

```
```

```
```

