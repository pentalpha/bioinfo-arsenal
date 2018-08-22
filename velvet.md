# Velvet
Sequence assembler for very short reads.

> Velvet is an algorithm package that has been designed to deal with de novo genome assembly and short read sequencing alignments. This is achieved through the manipulation of de Bruijn graphs for genomic sequence assembly via the removal of errors and the simplification of repeated regions.[2] Velvet has also been implemented inside of commercial packages, such as Sequencher, Geneious, MacVector and BioNumerics. (Wikipedia)

[User Manual](http://www.ebi.ac.uk/~zerbino/velvet/Manual.pdf)

## Installation

On Ubuntu, install using apt-get:
```sh
    $ sudo apt-get install velvet
```
## Using it

velveth generates the files that the assembler, velvetg, needs. 
```sh
    $ velveth <output_folder> hash_length [[-file_format][-read_type] <input files>
    $ velvetg <output_folder> [options]
```
One of the most important parameters is the hash-length (kmer) for the heuristics. In this data, bigger meant less contigs. The maximum is 31.

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

## More tutorials

- [Evomics, Learning, Assembly and Alignment](http://evomics.org/learning/assembly-and-alignment/velvet/);
- [Using the Velvet de novo assembler for short-read sequencing technologies, Daniel R. Zerbino](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2952100/);
