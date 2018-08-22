# abyss
> Usage: abyss-pe [OPTION]... [PARAMETER=VALUE]... [COMMAND]...

> Assemble reads into contigs and scaffolds. ABySS is a de novo sequence assembler intended for short paired-end reads and large genomes. See the abyss-pe man page for documentation of assembly parameters and commands. abyss-pe is a Makefile script, and so options of `make` may also be used with abyss-pe. See the `make` man page for documentation. (Command help argument)

> ABySS is a de novo, parallel, paired-end sequence assembler that is designed for short reads. The single-processor version is useful for assembling genomes up to 100 Mbases in size. The parallel version is implemented using MPI and is capable of assembling larger genomes. [Official Page](http://www.bcgsc.ca/platform/bioinfo/software/abyss)

## Installation
```sh
    $ sudo apt-get install abyss
```

## Using it, example

```sh
    $ mkdir abyss
    $ cd abyss/
    $ abyss-pe k=31 l=1 n=5 s=100 np=8 \
          name=Rhodo \
          lib='reads' \
          reads='../Rhodo_Hiseq_read1.fastq ../Rhodo_Hiseq_read2.fastq' \
          aligner=bowtie
```

## CLI Args
- k: "size of k-mer (when K is not set) or the span of a k-mer pair (when K is set)"; Instructions to find an optimal k value are provided at the project's [README](https://github.com/bcgsc/abyss#optimizing-the-parameter-k). Please read the section on k-mer values [here](Kmer.md);

- np: Number of processes to use;

- l: minimum alignment length of a read (bp, default=40);

- n: minimum number of pairs required for building contigs (default=10);

- s: minimum [unitig](https://github.com/mcveanlab/mccortex/wiki/unitig) size required for building contigs (bp, default=1000);

## Aftermath

[For information on scaffolds](https://genome.jgi.doe.gov/help/scaffolds.jsf)
