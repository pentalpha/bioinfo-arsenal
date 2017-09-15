# fastq-dump

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

That will leave us with the files:
Rhodo_Hiseq_read1.fastq  Rhodo_Hiseq_read2.fastq