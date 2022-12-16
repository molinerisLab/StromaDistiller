<<<<<<< HEAD
# Standard processing of RNA-seq from illumina NexSeq 500 runs

## Configuration

Install [miniconda](https://docs.conda.io/en/latest/miniconda.html).

Create a conda environment form the yml file contained in this repository
```conda env create -f local/share/data/conda_environment.yml```

Activate the environment
```conda activate bit_rnaseq_2.7```

Configure the [reference genome](https://bitbucket.org/irccit/gencode/src/master/).

Setup [bioinfotree](https://bitbucket.org/irccit/bit_docker/src/master/).

## Run

Clone the repo inside the run folder as downloaded form the NexSeq.
```
cd 190115_NS500140_0239_AHFLJCBGX5
git clone git@bitbucket.org:molineri/ngs_standard_processing.git BiT
```
Check and edit the params, in particular turn `DEMUX=Y` if you need demultiplexing
```
cd BiT/dataset/v1/
vi makefile
```
Put the `SampleSheet.csv` in the `local/share/data` dir
```
cp /tmp/SampleSheet.csv ../../local/share/data
```
Run the demultiplexing
```
bmake CORES=12 fastq/DEMUX.done
```
Basic QC
```
bmake -j 12 publish/multiqc_report.html
```
Aling and count
```
bmake CORES=6 -j 5 GEP.counts.gz
```
Normalize the counts
```
bmake GEP.counts.cpm.gz GEP.counts.tmm.gz
```
Rseqc
```
bmake -j 12 CORES=3 publish/multiqc_report.rseqc.html
```
# Single cell customization

Derive your branch from `scrnaseq_v0.1`, write a `metadata.txt` having at least these 3 column

1. Sample
1. Plate
1. WeelType

The following target produce the scRNA QC: `Rmarkdown/scrnaseq-qc.html`

See also https://bitbucket.org/molineri/ngs_standard_processing/src/scrnaseq_v0.1/README.md

# Changelog

## v0.1



###    Parametrization of STAR shared memory usage
Now is possible to change the way STAR use shared memory in the makefile or by command line, e.g. put `STAR_SHARED_MEMORY= NoSharedMemory` in makefile to avoid shared memory usage.

###    Automatic build of .bed file in gencode directory when needed
Two `extern` declaration to improve the first deployment of the pipeline.


###    Example of bioinfotree function to add in .bashrc 
In this example is shown how to configure the scratch and the tmp direcgtory
WIP unificarte TMPDIR and TMP_DIR

###    [conda environment] removed build indication and added sambamba
Minor: added the `rename` perl tool

=======
# StromaDistiller
>>>>>>> b6085b2a088d15b36bc9b590c0d6e7b99627459a
