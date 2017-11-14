FROM ubuntu:14.04
LABEL source "HaSAPPy"

# For nvidia-docker use:
#		FROM nvidia/cuda:6.5-runtime-ubuntu14.04

RUN apt-get update && apt-get install -y --no-install-recommends \
		ubuntu-desktop \
		build-essential cmake git \
		wget \
		python-dev python-numpy python-matplotlib python-scipy \
		python-sklearn python-pandas python-xlsxwriter \
		cython \
		idle \
		zlib1g-dev liblzma-dev libbz2-dev \
		python-pip
RUN pip install --upgrade pip && \
		pip install HTSeq


# Get bowtie2, unpack, and install into /usr/local/bin

RUN wget https://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.3.3.1/bowtie2-2.3.3.1-linux-x86_64.zip && \
		unzip bowtie2-2.3.3.1-linux-x86_64.zip && \
		mv bowtie2-2.3.3.1-linux-x86_64/ bowtie2/ && \
		rm bowtie2-2.3.3.1-linux-x86_64.zip
ENV PATH ${PATH}:/bowtie2

# Get NextGenMap source, build, and install into /usr/local/bin including openCL driver libs

RUN wget http://github.com/Cibiv/NextGenMap/archive/0.5.0.tar.gz && \
		tar -xzf 0.5.0.tar.gz && \
		mv NextGenMap-0.5.0/ NextGenMap/ && \
		rm 0.5.0.tar.gz && \
		cd NextGenMap && mkdir -p build && cd build && \
		cmake .. && make && \
		cp -r ../bin/ngm-0.5.0/* /usr/local/bin/ && \
		cd / && \
		rm -rf NextGenMap


# Copy nvBowtie and nvBWT executables to /usr/local/bin/

COPY nvBowtie /usr/local/bin/
COPY nvBWT /usr/local/bin/


# Get SRA Tooklit from NCBI

RUN wget https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.8.2-1/sratoolkit.2.8.2-1-ubuntu64.tar.gz && \
		tar -zxf sratoolkit.2.8.2-1-ubuntu64.tar.gz && \
		rm sratoolkit.2.8.2-1-ubuntu64.tar.gz
ENV PATH ${PATH}:/sratoolkit.2.8.2-1-ubuntu64/bin


# Get HaSAPPy from github --------------!!-- NEED TO ADD RefSeq_human_GRCh38-hg38.txt to /HaSAPPy/docs/

RUN cd / && \
		git clone https://github.com/gdiminin/HaSAPPy.git && \
		cd /HaSAPPy/program && \
		chmod +x PreprocessReads

# Generate annotation databases
RUN python /HaSAPPy/program/GeneReference_built.py -i /HaSAPPy/docs/mm10_REFSEQgenes.txt -o /HaSAPPy/docs/GeneReference_Mouse-MM10.pkl
# --- !!--- RUN python /HaSAPPy/program/GeneReference_built.py -i  /HaSAPPy/docs/RefSeq_human_GRCh38-hg38.txt -o /HaSAPPy/docs/GeneReference_Homo.pkl


# --------------- INIT -------------------------------


# Download reference sequences from iGenomes

RUN cd /HaSAPPy && mkdir reference && cd reference
RUN wget ftp://igenome:G3nom3s4u@ussd-ftp.illumina.com/PhiX/NCBI/1993-04-28/PhiX_NCBI_1993-04-28.tar.gz && \
		tar -xzf PhiX_NCBI_1993-04-28.tar.gz && \
		rm PhiX_NCBI_1993-04-28.tar.gz
RUN wget ftp://igenome:G3nom3s4u@ussd-ftp.illumina.com/Mus_musculus/UCSC/mm10/Mus_musculus_UCSC_mm10.tar.gz && \
		tar -xzf Mus_musculus_UCSC_mm10.tar.gz && \
		rm Mus_musculus_UCSC_mm10.tar.gz
RUN wget ftp://igenome:G3nom3s4u@ussd-ftp.illumina.com/Homo_sapiens/UCSC/hg38/Homo_sapiens_UCSC_hg38.tar.gz && \
		tar -xzf Homo_sapiens_UCSC_hg38.tar.gz && \
		rm Homo_sapiens_UCSC_hg38.tar.gz


# Download read datasets from SRA

RUN cd / && mkdir data && cd data && mkdir unselected && mkdir selected
RUN fastq-dump -O unselected/ --gzip SRR2064917
RUN fastq-dump -O unselected/ --gzip SRR2064920 
RUN fastq-dump -O unselected/ --gzip SRR2064924 
RUN fastq-dump -O unselected/ --gzip SRR2064927
RUN fastq-dump -O unselected/ --gzip SRR2064928
RUN fastq-dump -O unselected/ --gzip SRR2064929
RUN fastq-dump -O unselected/ --gzip SRR2064930
RUN fastq-dump -O selected/ --gzip SRR2064886
RUN fastq-dump -O selected/ --gzip SRR2064896 
RUN fastq-dump -O selected/ --gzip SRR2064898 
RUN fastq-dump -O selected/ --gzip SRR2064899
RUN fastq-dump -O selected/ --gzip SRR2064902
RUN fastq-dump -O selected/ --gzip SRR2064905
RUN fastq-dump -O selected/ --gzip SRR2064908


# Cleanup docker image

RUN apt-get purge -y --auto-remove build-essential cmake git && \
		rm -rf /var/lib/apt/lists/*

WORKDIR data/

CMD bash


WORKDIR data/

CMD bash
