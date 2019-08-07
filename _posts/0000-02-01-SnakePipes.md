# snakePipes
## A toolkit based on snakemake and python for analysis of NGS data

--

## Abstract

<small>
The scale and diversity of epigenomics data has been rapidly increasing and ever more studies now present analyses 
of data from multiple epigenomic techniques. 

Performing such integrative analysis is time-consuming, especially for exploratory research, 
since there are currently nopipelines available that allow fast processing of datasets from multiple epigenomic assays
 whilealso allow for flexibility in running or upgrading the workflows. 
 
Here we present a solution to this problem: snakePipes, 
which can process and perform downstream analysis of data from allcommon epigenomic techniques 
(ChIP-seq, RNA-seq, Bisulfite-seq, ATAC-seq, Hi-C andsingle-cell RNA-seq) in a single package.

 We demonstrate how snakePipes can simplifyintegrative analysis by reproducing and extending 
 the results from a recently publishedlarge-scale epigenomics study with a few simple commands. 

</small>

--

## URLS

- Github: [https://github.com/maxplanck-ie/snakepipes](https://github.com/maxplanck-ie/snakepipes)

- Docs: [https://snakepipes.readthedocs.io/en/latest/](https://snakepipes.readthedocs.io/en/latest/)

--

### Pipelines
<small>

| Pipeline   | Description  
|---|---
| [createIndices](https://snakepipes.readthedocs.io/en/latest/content/workflows/createIndices.html#createindices) | Create indices for an organism for further use within snakePipes 
| [DNA-mapping](https://snakepipes.readthedocs.io/en/latest/content/workflows/DNA-mapping.html#dna-mapping) | Basic DNA mapping using bowtie2, filter mapped files, QC and create coverage plots  
| [ChIP-seq](https://snakepipes.readthedocs.io/en/latest/content/workflows/ChIP-seq.html#chip-seq) | Use the DNA mapping output and run ChIP/Input normalization and peak calling
| [ATAC-seq](https://snakepipes.readthedocs.io/en/latest/content/workflows/ATAC-seq.html#atac-seq) | Use the DNA mapping output and detect open chromatin regions for ATAC-seq data
| [HiC](https://snakepipes.readthedocs.io/en/latest/content/workflows/HiC.html#hic) | Hi-C analysis workflow, from mapping to TAD calling
| [RNA-seq](https://snakepipes.readthedocs.io/en/latest/content/workflows/RNA-seq.html#rna-seq) | RNA-Seq workflow : From mapping to differential expression using DEseq2
| [scRNA-seq](https://snakepipes.readthedocs.io/en/latest/content/workflows/scRNA-seq.html#scrna-seq) | Single-cell RNA-Seq (CEL-Seq2) workflow : From mapping to differential expression
| [WGBS](https://snakepipes.readthedocs.io/en/latest/content/workflows/WGBS.html#wgbs) | Whole-genome Bisulfite-Seq analysis workflow, from mapping to DMR calling and differential methylation analysis

</small>

--

### Kick Off
<small>

0 - Run a container docker (optional)

```bash
docker pull continuumio/anaconda3 
docker run -v /path/to/workdir:/mnt/workdir -i -t continuumio/anaconda3 /bin/bash 
```
 
1 - Assuming you have python3 with conda, install snakePipes with:

```bash
conda create -n snakePipes -c mpi-ie -c bioconda -c conda-forge snakePipes
```

2 - Activate the appropriate conda environment.

```bash
conda activate snakePipes
```

3 - Create the various environments and register GATK.

```bash
snakePipes createEnvs
```

</small>

--

### Setup files

<small>

All files required to configure it would be installed in a default path. 
The path to these files can be displayed by running the following command:

```bash
snakePipes info
```

* **defaults.yaml** : Defines default tool and file paths.

* **cluster.yaml** : Defines execution command for the cluster. 
It contains both the default memory requirements as well as two options passed to snakemake that control 
how jobs are submitted to the cluster and files are retrieved

* **organisms/<organism>.yaml** : Defines genome indices and annotations for various organisms. 
(created automatically via the workflow `createIndices`.)

* **Workflow-specific defaults** : Defines default options for our command line wrappers.
Most of the default values could also be replaced from the command line wrappers while executing a workflow.

</small>

--

### Test Data

<small>

Test data for the various workflows is available at the following locations:

* [DNA mapping](https://zenodo.org/record/1346303)

* [ChIP-seq](https://zenodo.org/record/2624281)

* [ATAC-seq](https://zenodo.org/record/2624323)

* [RNA-seq](https://zenodo.org/record/2624408)

* [HiC](https://zenodo.org/record/2624479)

* [WGBS](https://zenodo.org/record/2624498)

* [scRNA-seq](https://zenodo.org/record/2624518)

</small>

--

### Examples

<small>

* Create Indices
```bash
createIndices [--local] --genomeURL <path/url to your genome fasta> --gtfURL <path/url to genes.gtf> -o <output_dir> <name>
```

* DNA Mapping
```bash
DNA-mapping [--local] -i <path to FASTQ dir> -o <output_dir> --trim --trim_prg cutadapt --fastqc --dedup --qualimap --DAG hg38
```

* WGBS
```bash
WGBS [--local] --DAG --trim user --sampleSheet <sampleSheet.tsv> -i <path to FASTQ dir> -o <output_dir> mm10
```

* RNA-Seq
```bash
RNA-seq [--local] --sampleSheet <sampleSheet.tsv> -m alignment --DAG -i <path to FASTQ dir> -o <output_dir>  mm10
```

</small>

--

# ...


