# Time for a real case: Identify candidates involved in X chromosome inactivation

In this tutorial, we are going to perform an anlysis with HaSAPPy software on true datasets derived from an haploid screening in ESCs to identify new modulators of X chromosome inactivation. For the details of the screening refer to Monfort et al. 2015 (https://www.ncbi.nlm.nih.gov/pubmed/26190100)

## Prerequisites
1. Installation of HaSAPPy package
2. Necessary Python packages correctly updated
3. Installation of Bowtie2
4. Download of reference genomes for the alignment programs
5. Maked executable the PreprocessReads
6. Built the gene annotation database using GeneReference_built.py
6. Performed the Test_HaSAPPy_installation tutorial

The working directory is
```
Users/User/HaSAPPy
```

Your working directory should look like that:
```
|- HaSAPPy
    |- reference
        |- Phix
            |- ...
        |- Mus_musculus
            |- ...
    |- experiments
        |- ...
    |- program
        |- …
    |- docs
        |- LoadModule.txt
        |- mm10REFSEQgenes.txt
        |- GeneReference_mouse_mm10.pkl
        |- Tutorials
             |- ...
        |- test
             |- ...
```         

## Collecting FASTQ file from the SRA repository
We will use data from the SRA database associated with the haploid ES cell screen and store the read FASTQ files in a data folder in a selected/ and unselected/ subfolder.

```
cd ..
mkdir data
cd data
mkdir unselected
mkdir selected
