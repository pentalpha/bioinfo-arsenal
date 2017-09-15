# prefetch

[Official Reference](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=prefetch)

A tool from the NCBI's SRA Toolkit. It allows command-line downloading of SRA, dbGaP, and ADSP data.

To install the whole sra-toolkit:

```sh
    $ sudo apt-get install sra-toolkit
```

## SRA, dbGaP, and ADSP data
> Sequence Read Archive (SRA) makes biological sequence data available to the research community to enhance reproducibility and allow for new discoveries by comparing data sets. The SRA stores raw sequencing data and alignment information from high-throughput sequencing platforms, including... (NCBI)

> The database of Genotypes and Phenotypes (dbGaP) was developed to archive and distribute the data and results from studies that have investigated the interaction of genotype and phenotype in Humans. (NCBI)

> ADSP: Alzheimer's Disease Sequencing Project.

## Default usage

Usage:

```sh
    $ prefetch [options] <path/SRA file | path/kart file> [<path/file> ...]
    $ prefetch [options] <SRA accession>
    $ prefetch [options] --list <kart_file>
```

## applycation

In this example, we will download the SRR522243 and ......245 SRA data:

### SRR552224X

> Illumina whole genome shotgun sequencing of genomic DNA paired-end library 'Solexa-42867' containing sample Rhodobacter LW1. [URL](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=run_browser&run=SRR522243)

### Downloading the data

```sh
    $ prefetch SRR522243
    $ prefetch SRR522245
```
