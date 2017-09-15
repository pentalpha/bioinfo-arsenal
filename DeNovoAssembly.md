# What is Sequence Assembly?

> In bioinformatics, sequence assembly refers to aligning and merging fragments from a longer DNA sequence in order to reconstruct the original sequence. This is needed as DNA sequencing technology cannot read whole genomes in one go, but rather reads small pieces of between 20 and 30000 bases, depending on the technology used. Typically the short fragments, called reads, result from shotgun sequencing genomic DNA, or gene transcript (ESTs). (Wikipedia)

> The problem of sequence assembly can be compared to taking many copies of a book, passing each of them through a shredder with a different cutter, and piecing the text of the book back together just by looking at the shredded pieces. (Wikipedia again)

# A Guide For DeNovo Sequence Assembly

Overview of the tools we are gonna use here:

1. [prefetch](DeNovoAssembly.md#1-prefetch) 
2. [fastq-dump](DeNovoAssembly.md#2-fastq-dump)  
3. [Option A: spades](DeNovoAssembly.md#3-option-a-spades)
4. [Option B: abyss](DeNovoAssembly.md#4-option-b-abyss)
5. [Option C: Velvet](DeNovoAssembly.md#5-option-c-velvet)
6. [Analysing results](DeNovoAssembly.md#6-analysing-the-results)

# 1. prefetch
To install along with the whole sra-toolkit:

```sh
    $ sudo apt-get install sra-toolkit
```

## 1.1. Applycation

In this example, we will download the SRR522243 and ......245 SRA data:

### 1.1.1. SRR552224X

> Illumina whole genome shotgun sequencing of genomic DNA paired-end library 'Solexa-42867' containing sample Rhodobacter LW1. [URL](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=run_browser&run=SRR522243)

### 1.1.2. Downloading the data

```sh
    $ prefetch SRR522243
    $ prefetch SRR522245
```
The files are not downloaded to the current directory. Where are they? I have no idea.

[For more info](prefatch.md)

# 2. fastq-dump

Basically, it is one of the '*-dump' utilities in sra-toolkit that converts SRA data to other formats.
```sh
    $ fastq-dump --split-files SRR522243
    $ fastq-dump --split-files SRR522245
```
It will create the following .fastq files on the current directory:
SRR522243_2.fastq  SRR522245_2.fastq
SRR522243_1.fastq  SRR522245_1.fastq

Probably because of the --split-files arg., there are _1 and _2 files. These are the two nucleotide strands. #TODO

Uniting the pair fastq's in a single file:
```sh
    $ cat SRR522243_1.fastq  SRR522245_1.fastq > Rhodo_Hiseq_read1.fastq
    $ cat SRR522243_2.fastq  SRR522245_2.fastq > Rhodo_Hiseq_read2.fastq
    $ mkdir raw 
    $ mv *_[12].fastq raw/
```

That will leave us with the files:
Rhodo_Hiseq_read1.fastq  Rhodo_Hiseq_read2.fastq

[For more info](fastq-dump.md)

# 3. Option A: spades

Spades is a russian genome assembler writen in Python. He takes the raw fastq data and generates assembled .fasta files.
[Official Webpage](http://cab.spbu.ru/software/spades/)

## 3.1 Instalation
```sh
    $ sudo apt-get install spades
```

## 3.2 Using it

```sh
    $ spades.py -t 8 --pe1-1 Rhodo_Hiseq_read1.fastq --pe1-2 Rhodo_Hiseq_read2.fastq -o SpadesOut
```

Created around 3100 contigs

[For more info](spades.md)

# 4. Option B: abyss

## 4.1. Installation
```sh
    $ sudo apt-get install abyss
```

## 4.2. Using it

```sh
    $ mkdir abyss
    $ cd abyss/
    $ abyss-pe k=31 l=1 n=5 s=100 np=8 \
          name=Rhodo \
          lib='reads' \
          reads='../Rhodo_Hiseq_read1.fastq ../Rhodo_Hiseq_read2.fastq' \
          aligner=bowtie
```

## 4.4. Aftermath
From Rhodo-stats.html:

n   n:500   L50     min     N80     N50     N20     E-size  max     sum     name

17918   1183    424     500     594     796     1177    943     4677    943532  Rhodo-unitigs.fa

17722   1220    430     500     609     843     1232    977     4677    1009253     Rhodo-contigs.fa

17634   1136    346     500     609     843     1911    1509    10908   1009260     Rhodo-scaffolds.fa

[For information on scaffolds](https://genome.jgi.doe.gov/help/scaffolds.jsf)

[For more info](abyss.md)

# 5. Option C: Velvet
Sequence assembler for very short reads.

## 5.1. Installation

On Ubuntu, install using apt-get:
```sh
    $ sudo apt-get install velvet
```
## 5.2. Using it

```sh
    $ velveth velvet/ 31 -shortPaired -fastq -separate Rhodo_Hiseq_read1.fastq Rhodo_Hiseq_read2.fastq
    $ velvetg velvet/ -cov_cutoff auto
    $ more velvet/contigs.fa
```
With hash-length 21, the commands above created 225000 contigs. With 27, 60800. Finally, with hash_length 31 and -cov_cutoff auto, we achieved 11200 contigs.

Using additional parameters:
```sh
    $ velvetg velvet/ -cov_cutoff 6 -exp_cov auto
        Final graph has 10172 nodes and n50 of 7045, max 26197, total 1448145, using 645018/1979216 reads
```
Now, with the -exp_cov parameter, velvetg proceded to scaffolding the resulting contigs. The coverage cutoff 6 removed contigs with less than 6x.

[For more info](velvet.md)

# 6. Analysing the results
Copy everything to a single directory:
```sh
    $ mkdir genomes
    $ cp SpadesOut/scaffolds.fasta genomes/spades.sc.fa
    $ cp abyss/Rhodo-scaffolds.fa genomes/abyss.sc.fa
    $ cp velvet/contigs.fa genomes/velvet.sc.fa
```

## 6.1. Quast.py
> QUAST-a quality assessment tool for evaluating and comparing genome assemblies. This tool improves on leading assembly comparison software with new ideas and quality metrics. QUAST can evaluate assemblies both with a reference genome, as well as without a reference. QUAST produces many reports, summary tables and plots to help scientists in their research and in their publications. [QUAST: quality assessment tool for genome assemblies](https://www.ncbi.nlm.nih.gov/pubmed/23422339);

Download from [sourceforge](http://quast.sourceforge.net/quast).

```sh
    $ quast.py -o quast_genomes/ genomes/*
    $ xdg-open quast_genomes/icarus.html
```

[For more info](quast.md)

# Closing gaps: 
not yet
