# Performing a test run of your HaSAPPy installation

This tutorial shows how to quickly verify that HaSAPPy is correctly installed on your system and all dependencies are met. For testing the core functionality of HaSAPPy a pre-filled **LoadModule.txt** command script and a **Sequence_test** file are included in the docs/test folder in the installation.
It is recommended to run this text for a new installation before starting to use HaSAPPy for data analysis workflows.

The working directory for this tutorial is:

```
/Users/User
```

## Prerequisites

1. Installation of HaSAPPy package
2. Necessary Python packages correctly updated
3. Installation of bowtie2 

### Create a folder where the experiments and run information will be stored
The folder structure should look like this:
```
/Users/User/
|
└── HaSAPPy
    ├── ...
    └── experiments
        └── test
            └── ...
```

### Download and unpack sequence archives for PhiX and Mus Musculus genomes
Add a **reference/** folder where the genome sequences and indices will be stored:

```
/Users/User/
|
└── HaSAPPy
    ├── ...
    └── experiments
    |   └── test
    |       └── ...
    └── reference
	    └── ...
```

Genome sequences of many common species are provided at:

http://support.illumina.com/sequencing/sequencing_software/igenome.html

Using a web browser download the PhiX genome (NCBI 1993-04-28) and the mouse genome (GCRm38/mm10) to your ~/HaSAPPy/reference folder. Then unpack the archives in the terminal:

```
tar -xzvf PhiX_NCBI_1993-04-28.tar.gz
tar -xzvf Mus_musculus_UCSC_mm10.tar.gz
```


### Self execute permission on PreprocessReads
 
Enter in the HaSAPPy folder and prepare the program

```
cd HaSAPPy
cd program
chmod +x PreprocessReads 
```

###Build the gene annotation database using GeneReference_built.py
A gene annotation file for the mouse genome (GCRm38/mm10) is supplied with the source. If you need other genomes suitable annotation files can be obtained from the UCSC genome browser (http://genome.ucsc.edu/cgi-bin/hgTables?command=start). 
Read the Generate_human_gene_refernce.md tutorial for a detailed exemple

```
python GeneReference_built.py -i /Users/User/HaSAPPy/docs/mm10_REFSEQgenes.txt -o /Users/User/HaSAPPy/docs/GeneReference_mouse_mm10.pkl
```

A new file GeneReference_mouse_mm10.pkl should appear in the current folder.

Now we are ready to start. Your working directory (/Users/User/HaSAPPy) should have this structure

* the HaSAPPy directory

```
|- HaSAPPy
    |- reference
        |- ...
    |- experiments
        |- ...
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
             |- Aligned.sam
             |- Sequence.fastq
             |- LoadModule_test_from_Trim.txt
             |- LoadModule_test_from_IIdefinition.txt
```

* the reference directory

```
|- reference
    |- Phix
        |- NCBI
	   |- 1993-04-28
	       |- Annotation
	          |- ...
	       |- Sequence
	          |- ...
		  |- BowtieIndex
		      |- genome.1.ebwt
		      |- ...
    |- Mus_musculus
        |- UCSC
	   |- mm10
	       |- Annotation
	          |- ...
	       |- Sequence
	          |- ...
		  |- BowtieIndex
		      |- genome.1.ebwt
		      |- ...		      
```

* the experiments directory

```
|- experiments
    |- test
        |- ...
```

##Compile the LoadModule
In this tutorial, we are going to test the functionality of the HaSAPPy packages (starting from the Trim.py module) using the LoadModule_test_from_Trim.txt and the Sequence.fastq files available in the docs folder.

```
/Users/User/HaSAPPy/docs/test
```

In addition, you can test HaSAPPy performance form the IIdefinition.py module using the LoadModule_test_from_IIdefinition.txt and the Aligned.sam files

Open the LoadModule_test_from_Trim.txt file using a text editor. As you can see most of the tasks have already been filled out. You need to fill those tasks related to the location of files according to your home directory.

* Section 1
   * Task 1A and 1B

```
Operator Name: 
@1A) User
Storing location (provide a correct path):
@1B) /Users/User/HaSAPPy/experiments/test
```
* Section 2
  * 2D

```
Location of input file 1 (add additional lines if necessary):
@2D) /Users/User/HaSAPPy/docs/test/Sequence.fastq
@2D) /Users/User/HaSAPPy/docs/test/Sequence.fastq
```

* Section 3
  *  3A

```
Location of Phix reference genome:
@3A)/Users/User/HaSAPPy/reference/Phix/NCBI/1993-04-28/Sequence/BowtieIndex/genome
```

Bowtie2 dosen't request extension of the file "genome"

* Section 4
  * 4B

```
Location of reference genome:
@4B) /Users/User/HaSAPPy/reference/Mus_musculus/UCSC/mm10/Sequence/BowtieIndex/genome
```

* Section 6
  * 6A

```
Location of gene reference:
@6A)/Users/User/HaSAPPy/docs/GeneReference_mouse_mm10.pkl
```

* Section 9
  * 9C

```
FOR Plot I.I. in gene models:
	Location of gene reference:
	@9C)/Users/User/HaSAPPy/docs/GeneReference_mouse_mm10.pkl
```

Save the file and copy the file name to the clipboard

##Run the test
Move into the HaSAPPy program folder

```
cd /Users/User/HaSAPPy/program
```

Start the analysis  HaSAPPY_start.py providing as input the LoadModule_test_from_Trim.txt

```
python HaSAPPY_start.py /Users/User/HaSAPPy/docs/test/LoadModule_test_from_Trim.txt
```

If everything is correctly installed the analysis should finish without any errors

##Inspect the output
In the /Users/User/HaSAPPy/experiments/test the following files and folders should have been created

```
|- test
    |- test_1_yyyy-mm-dd
        |- test_1_info.txt
        |- graph
        |- raw
            |- test_1_Aligned.sam
            |- ...
            |- test_1_IIRawdata.pkl
    |- test_2_yyyy-mm-dd
        |- test_2_info.txt
        |- graph
        |- raw
            |- test_2_Aligned.sam
            |- ...
            |- test_2_IIRawdata.pkl
    |- Analysis
        |- yyyy-mm-dd
            |- analysis_info.txt
            |- graph
                |- Xist_SelectedvsControl.svg
                |- ...
            |- raw
                |- GroupAnalysis.pkl
                |- RawData.pkl
            |- Table_yyyy-mm-dd.xlsx
```

Exploring the Table_yyyy-mm-dd.xlsx and the Xist_SelectedvsControl.svg files you should aspect the following output:

![alt text] (https://github.com/gdiminin/HaSAPPy/blob/master/docs/Tutorials/Figures/Test_HaSAPPy_installation.png)

If this is what you see, your HaSAPPy installation is working correctly! Have fun!!!!


[**RETURN TO THE MAIN PAGE**](https://github.com/gdiminin/HaSAPPy/blob/master/README.md)
