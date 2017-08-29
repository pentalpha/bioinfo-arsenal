# What is Sequence Assembly?
> In bioinformatics, sequence assembly refers to aligning and merging fragments from a longer DNA sequence in order to reconstruct the original sequence. This is needed as DNA sequencing technology cannot read whole genomes in one go, but rather reads small pieces of between 20 and 30000 bases, depending on the technology used. Typically the short fragments, called reads, result from shotgun sequencing genomic DNA, or gene transcript (ESTs). (Wikipedia)

> The problem of sequence assembly can be compared to taking many copies of a book, passing each of them through a shredder with a different cutter, and piecing the text of the book back together just by looking at the shredded pieces. (Wikipedia again)

# A Pipeline For De Novo Sequence Assembly
Overview of the tools we are gonna use here:

1- prefetch 
2- fastq-dump  
3- spades
4- abyss
5- SOAPdenovo
6- MaSuRCA
7- redundans
8- quast

# SRA, dbGaP, and ADSP data
> Sequence Read Archive (SRA) makes biological sequence data available to the research community to enhance reproducibility and allow for new discoveries by comparing data sets. The SRA stores raw sequencing data and alignment information from high-throughput sequencing platforms, including... (NCBI)

> The database of Genotypes and Phenotypes (dbGaP) was developed to archive and distribute the data and results from studies that have investigated the interaction of genotype and phenotype in Humans. (NCBI)

> ADSP: Alzheimer's Disease Sequencing Project

#1. prefetch
(Official Reference)[https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=prefetch]
A tool from the NCBI's SRA Toolkit. It allows command-line downloading of SRA, dbGaP, and ADSP data.
To install the whole sra-toolkit:
```sh
    $ sudo apt-get install sra-toolkit
```

##1.1. SRA, dbGaP, and ADSP data
> Sequence Read Archive (SRA) makes biological sequence data available to the research community to enhance reproducibility and allow for new discoveries by comparing data sets. The SRA stores raw sequencing data and alignment information from high-throughput sequencing platforms, including... (NCBI)

> The database of Genotypes and Phenotypes (dbGaP) was developed to archive and distribute the data and results from studies that have investigated the interaction of genotype and phenotype in Humans. (NCBI)

> ADSP: Alzheimer's Disease Sequencing Project.

##1.2. Default usage
Usage:
```sh
    $ prefetch [options] <path/SRA file | path/kart file> [<path/file> ...]
    $ prefetch [options] <SRA accession>
    $ prefetch [options] --list <kart_file>
```

##1.3. Applycation
In this example, we will download the SRR522243 and ......245 SRA data:

###1.3.1. SRR552224X
> Change accession... Illumina whole genome shotgun sequencing of genomic DNA paired-end library 'Solexa-42867' containing sample Rhodobacter LW1. [URL](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=run_browser&run=SRR522243)

###1.3.2. Downloading the data
```sh
    $ prefetch SRR522243
    $ prefetch SRR522245
```
The files are not downloaded to the current directory. Where are they? I have no idea.

#2. fastq-dump
> NCBI’s fastq-dump has to be one of the worst-documented programs available online. The default parameters for fastq-dump are also ridiculous and certainly not what you want to use. They also have absolutely required parameters mixed in with totally optional parameters, and so you have no idea what is required and what is optional. [A, very angry with the original docs, overview of fastq-dump](https://edwards.sdsu.edu/research/fastq-dump/)

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

Why not just omit the --split-files and have one file, to begin with? I Also have no idea. #TODO

That will leave us with the files:
Rhodo_Hiseq_read1.fastq  Rhodo_Hiseq_read2.fastq

#3. spades
[Official Webpage](http://cab.spbu.ru/software/spades/)

#TODO