# Using SRA tools with SLURM/JIC HPC

The SRA (sequenced read archive) toolkit is a way of downloading published data from NCBI. https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=announcement

This guide will explain how to download data using the command line on the HPC
```
prefetch SRR3311812
```

## installation/toolkit

First, open a software node by typing 'software' in a PuTTY window. Then, in your home directory type to download and uncompress;

``` 
wget http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/current/sratoolkit.current-centos_linux64.tar.gz
tar xvzf sratoolkit.current-centos_linux64.tar.gz 
```
This should create a folder called ~/sratoolkit.3.0.0-centos_linux64 

in the bin folder there should be a range of functions. We'll use 'prefetch' and 'fastq-dump'

## software node and prefetch

This bit gets a little fiddly as only the software node can communicate with the outside world but we want to download the data to either the group folder or scratch space. To get round that I'm going to use two different PuTTY windows, one software and one interactive.

On the software node:

```
~/sratoolkit.3.0.0-centos_linux64/bin/prefetch SRR3311812
```
Please note that the version of sratoolkit (sratoolkit.3.0.0) may need to be changed overtime.
This should create a directory named SRR3311812 and a file called SRR3311812.sra.

We can then proceed to extracting the fastq files.

There are versions of sratools installed on the hpc, e.g.

```
source sratoolkit-2.9.0
```

## fastq-dump

On the interactive node:

I want to perform this RNA-seq analysis in my scratch space so I need to use an interactive node to extract the fastq files from the folder in my home directory.
```
cd ncbi/public/sra/
~/sratoolkit.3.0.0-centos_linux64/bin/fastq-dump --split-files --outdir ~/myscratch/tRNA/ SRR3311812/SRR3311812.sra
```
Remove the sra file after extracting fastq file to save the space.
This will create a file called SRR3311812_1.fastq in the desired output directory. The --split-files option is needed for paired end read data but doesn't affect single end data, so is a good default option to have.

If you're downloading multiple SRA files, there will be a lot of switching between nodes. Once you're done though the data is ready for downstream analysis. 
