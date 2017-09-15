# What is Sequence Assembly?

> In bioinformatics, sequence assembly refers to aligning and merging fragments from a longer DNA sequence in order to reconstruct the original sequence. This is needed as DNA sequencing technology cannot read whole genomes in one go, but rather reads small pieces of between 20 and 30000 bases, depending on the technology used. Typically the short fragments, called reads, result from shotgun sequencing genomic DNA, or gene transcript (ESTs). (Wikipedia)

> The problem of sequence assembly can be compared to taking many copies of a book, passing each of them through a shredder with a different cutter, and piecing the text of the book back together just by looking at the shredded pieces. (Wikipedia again)

# A Guide For DeNovo Sequence Assembly

Overview of the tools we are gonna use here:

1. [prefetch](DeNovoAssembly.md#) 
2. [fastq-dump](DeNovoAssembly.md#)  
3. [Option A: spades](DeNovoAssembly.md#)
4. [Option B: abyss](DeNovoAssembly.md#)
5. [Option C: Velvet](DeNovoAssembly.md#)
6. [Analysing results](DeNovoAssembly.md#)

# 1. prefetch

[Official Reference](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=prefetch)

A tool from the NCBI's SRA Toolkit. It allows command-line downloading of SRA, dbGaP, and ADSP data.

To install the whole sra-toolkit:

```sh
    $ sudo apt-get install sra-toolkit
```

## 1.1. SRA, dbGaP, and ADSP data
> Sequence Read Archive (SRA) makes biological sequence data available to the research community to enhance reproducibility and allow for new discoveries by comparing data sets. The SRA stores raw sequencing data and alignment information from high-throughput sequencing platforms, including... (NCBI)

> The database of Genotypes and Phenotypes (dbGaP) was developed to archive and distribute the data and results from studies that have investigated the interaction of genotype and phenotype in Humans. (NCBI)

> ADSP: Alzheimer's Disease Sequencing Project.

## 1.2. Default usage

Usage:

```sh
    $ prefetch [options] <path/SRA file | path/kart file> [<path/file> ...]
    $ prefetch [options] <SRA accession>
    $ prefetch [options] --list <kart_file>
```

## 1.3. Applycation

In this example, we will download the SRR522243 and ......245 SRA data:

### 1.3.1. SRR552224X

> Illumina whole genome shotgun sequencing of genomic DNA paired-end library 'Solexa-42867' containing sample Rhodobacter LW1. [URL](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=run_browser&run=SRR522243)

### 1.3.2. Downloading the data

```sh
    $ prefetch SRR522243
    $ prefetch SRR522245
```
The files are not downloaded to the current directory. Where are they? I have no idea.

# 2. fastq-dump

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

# 3. Option A: spades

Spades is a russian genome assembler writen in Python. He takes the raw fastq data and generates assembled .fasta files.
[Official Webpage](http://cab.spbu.ru/software/spades/)

> SPAdes (St. Petersburg genome assembler)[1] is a genome assembly algorithm which was designed for single cell and multi-cells bacterial data sets. However, it might not be suitable for large genomes projects.  (Wikipedia)

Its made for facterial data sets. Not indicated for large genomes.

## 3.1 Instalation
```sh
    $ sudo apt-get install spades
```

## 3.2 Using it

```sh
    $ spades.py -t 8 --pe1-1 Rhodo_Hiseq_read1.fastq --pe1-2 Rhodo_Hiseq_read2.fastq -o SpadesOut
```

This will create the directory:
```sh
    SpadesOut/
    ├── corrected
    │   └── configs
    ├── K21
    │   └── configs
    │       ├── datasets
    │       └── datasets_archive
    ├── K33
    │   └── configs
    │       ├── datasets
    │       └── datasets_archive
    ├── K55
    │   ├── configs
    │   │   ├── datasets
    │   │   └── datasets_archive
    │   └── path_extend
    ├── misc
    └── tmp
    (17 directories)
```

Created around 3100 contigs

# 4. Option B: abyss
> Usage: abyss-pe [OPTION]... [PARAMETER=VALUE]... [COMMAND]...

> Assemble reads into contigs and scaffolds. ABySS is a de novo sequence assembler intended for short paired-end reads and large genomes. See the abyss-pe man page for documentation of assembly parameters and commands. abyss-pe is a Makefile script, and so options of `make` may also be used with abyss-pe. See the `make` man page for documentation. (Command help argument)

> ABySS is a de novo, parallel, paired-end sequence assembler that is designed for short reads. The single-processor version is useful for assembling genomes up to 100 Mbases in size. The parallel version is implemented using MPI and is capable of assembling larger genomes. [Official Page](http://www.bcgsc.ca/platform/bioinfo/software/abyss)

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

## 4.3. CLI Args
- k: "size of k-mer (when K is not set) or the span of a k-mer pair (when K is set)"; Instructions to find an optimal k value are provided at the project's [README](https://github.com/bcgsc/abyss#optimizing-the-parameter-k). Please read the section on k-mer values [here](Kmer.md);

- np: Number of processes to use;

- l: minimum alignment length of a read (bp, default=40);

- n: minimum number of pairs required for building contigs (default=10);

- s: minimum [unitig](https://github.com/mcveanlab/mccortex/wiki/unitig) size required for building contigs (bp, default=1000);

## 4.4. Aftermath
From Rhodo-stats.html:

n   n:500   L50     min     N80     N50     N20     E-size  max     sum     name

17918   1183    424     500     594     796     1177    943     4677    943532  Rhodo-unitigs.fa

17722   1220    430     500     609     843     1232    977     4677    1009253     Rhodo-contigs.fa

17634   1136    346     500     609     843     1911    1509    10908   1009260     Rhodo-scaffolds.fa

[For information on scaffolds](https://genome.jgi.doe.gov/help/scaffolds.jsf)

# 5. Option C: Velvet
Sequence assembler for very short reads.

> Velvet is an algorithm package that has been designed to deal with de novo genome assembly and short read sequencing alignments. This is achieved through the manipulation of de Bruijn graphs for genomic sequence assembly via the removal of errors and the simplification of repeated regions.[2] Velvet has also been implemented inside of commercial packages, such as Sequencher, Geneious, MacVector and BioNumerics. (Wikipedia)

[User Manual](http://www.ebi.ac.uk/~zerbino/velvet/Manual.pdf)

## 5.1. Installation

On Ubuntu, install using apt-get:
```sh
    $ sudo apt-get install velvet
```
## 5.2. Using it

velveth generates the files that the assembler, velvetg, needs. 
```sh
    $ velveth <output_folder> hash_length [[-file_format][-read_type] <input files>
    $ velvetg <output_folder> [options]
```
The most important parameter is the hash-length for the heuristics. In this data, bigger meant less contigs. The maximum is 31.

> As is often the case, it’s a tradeoff between specificity and sensitivity. Longer kmers bring you more specificity (i.e. less spurious overlaps) but lowers coverage (cf. below). . . so there’s a sweet spot to be found with time and experience. Experience shows that kmer coverage should be above 10 to start getting decent results. If Ck is above 20, you might be “wasting” coverage. Experience also shows that empirical tests with different values for k are not that costly to run! (User Manual, section 5.2)

```sh
    $ velveth velvet/ 31 -shortPaired -fastq -separate Rhodo_Hiseq_read1.fastq Rhodo_Hiseq_read2.fastq
    $ velvetg velvet/ -cov_cutoff auto
    $ more velvet/contigs.fa
```
With hash-length 21, the commands above created 225000 contigs. With 27, 60800. Finally, with hash_length 31 and -cov_cutoff auto, we achieved 11200 contigs.

Coverage Cutoff is used to cut out of the results any small contig which is likely to be an error. The auto value is for velvet to choose this cutoff value.

Using additional parameters:
```sh
    $ velvetg velvet/ -cov_cutoff 6 -exp_cov auto
        Final graph has 10172 nodes and n50 of 7045, max 26197, total 1448145, using 645018/1979216 reads
```
Now, with the -exp_cov parameter, velvetg proceded to scaffolding the resulting contigs. The coverage cutoff 6 removed contigs with less than 6x.

## 5.3. More tutorials

- [Evomics, Learning, Assembly and Alignment](http://evomics.org/learning/assembly-and-alignment/velvet/);
- [Using the Velvet de novo assembler for short-read sequencing technologies, Daniel R. Zerbino](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2952100/);

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

# Closing gaps: 
not yet
