# spades

Spades is a russian genome assembler writen in Python. He takes the raw fastq data and generates assembled .fasta files.
[Official Webpage](http://cab.spbu.ru/software/spades/)

> SPAdes (St. Petersburg genome assembler)[1] is a genome assembly algorithm which was designed for single cell and multi-cells bacterial data sets. However, it might not be suitable for large genomes projects.  (Wikipedia)

Its made for facterial data sets. Not indicated for large genomes.

## Instalation
```sh
    $ sudo apt-get install spades
```

## Using it

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