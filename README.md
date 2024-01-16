# CutandRun
 CUT&RUN-Flow (CnR-flow), a Nextflow pipeline for QC, tag trimming, normalization, and peak calling for paired-end sequencing data from CUT&RUN experiments. This software is available via GitHub, at http://www.github.com/RenneLab/CnR-flow.
 Full project documentation is available at CUT&RUN-Flow's https://cnr-flow.readthedocs.io/#
 
CUT&RUN-Flow was built by using Nextflow, a powerful domain-specific workflow language built to create flexible and efficient bioinformatics pipelines. Nextflow provides extensive flexibility in utilizing cluster computing environments such as PBS and SLURM, and in automated and compartmentalized handling of dependencies using Conda / Bioconda, Docker, Singularity or Environment Modules.

The pipeline is built using Nextflow, a workflow tool to run tasks across multiple compute infrastructures in a portable, reproducible manner. It is capable of using containerisation and package management making installation trivial and results highly reproducible. The Nextflow DSL2 implementation of this pipeline uses one container per process, which makes it easier to maintain and update software dependencies. Where possible, these processes have been submitted to and installed from nf-core/modules.

The pipeline has been developed with continuous integration (CI) and test driven development (TDD) at its core. nf-core code and module linting as well as a battery of over 100 unit and integration tests run on pull request to the main repository and on release of the pipeline. On official release, automated CI tests run the pipeline on a full-sized dataset on AWS cloud infrastructure. This ensures that the pipeline runs on AWS, has sensible resource allocation defaults set to run on real-world datasets, and permits the persistent storage of results to benchmark between pipeline releases and other analysis sources. The results obtained from the full-sized test can be viewed on the nf-core website.
## Cutandrun flow diagram
The following flow diagram is a flow of running cutandrun by performing nextflow:
![Cutandrun flow diagram](https://github.com/nf-core/cutandrun/blob/master/docs/images/cutandrun-flow-diagram-v3.0.png)
 
## Install nextflow with conda 
1. Create evironment with python 2.7 in Linux server such as scccbbsr.cgcent. If Linux server does not install anaconda, then you need to download  Miniconda3-latest-Linux-x86_64.sh:
 ```  
   $ mkdir -p ~/miniconda3
   
   $ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
   
   $ bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
   
   $ rm -rf ~/miniconda3/miniconda.sh
   ```
After installing, initialize your newly-installed Miniconda. The following commands initialize for bash and zsh shells:
   ```
$ ~/miniconda3/bin/conda init bash
$ ~/miniconda3/bin/conda init zsh
   ```
Then use conda to create environment:
   ```
 $ conda create -n python2.7 python=2.7 anaconda
   ```
Activate environment python2.7 with conda
```
(base)[[yxt477@scccbbsr Yuande]]$ conda activate python2.7
```
In enviroment python2.7, use conda to install nexflow from bioconda:
```
(python2.7) [yxt477@scccbbsr Yuande]$ conda install -c bioconda nextflow
```
Another way is to install nextflow in base environment in your Linux server account:
```
(base) [yxt477@scccbbsr Yuande]$conda install -c bioconda nextflow
```
You don't need to install cutandrun pipeline because cutandrun pipeline was installed in nextflow when nextflow is installed.
## CutandRun pipeline provides the following performing 16 processes or tools:

1. Merge re-sequenced FastQ files([cat](cat))

2. Read QC([FastQC](FastQC))

3. Adapter and quality trimming([Trim Galore!](TrimGalore!))

4. Alignment to both target and spike-in genomes([Bowtie2](Bowtie2))

5. Filter on quality, sort and index alignments([samtools](samtools))

6. Duplicate read marking ([picard](picard))

7. Create bedGraph files ([bedtools](bedtools))

8. Create bigWig coverage files ([bedGraphToBigWig](bedGraphToBigWig))

9. Peak calling ([SEACR](SEACR), [MACS2](MACS2))

10. Consensus peak merging and reporting ([bedtools](bedtools))

11. Library complexity ([preseq](preseq)(Preseq | The Smith Lab))

12. Fragment-based quality control ([deepTools](deepTools))

13. Peak-based quality control ([bedtools](bedtools), custom python)

14. Heatmap peak analysis ([deepTools](deepTools))

15. Genome browser session ([IGV](IGV))

16. Present all QC in web-based report ([MultiQC](MultiQC))

## Make sample sheet with csv format

After that, you need make an samplesheet.csv. Here an example is a templet to make the samplesheet.csv where the column title must be group,replicate,fastq_1,fastq_2,control. 
```
group,replicate,fastq_1,fastq_2,control
S90,1,/data1/Yuande/DNA_cutandrun/data2/S90_R1_001.fastq.gz,/data1/Yuande/DNA_cutandrun/data2/S90_R2_001.fastq.gz,FOXA1
FOXA1,1,/data1/Yuande/DNA_cutandrun/data2/S89_R1_001.fastq.gz,/data1/Yuande/DNA_cutandrun/data2/S89_R2_001.fastq.gz,
```
The first column in samplesheet.csv is group. Here S90 is name of group in which there may be one or multiple cutandrun samples. You may use the other group name. A group is  an antibody. An experiment may have multiple antibodies. Each antibody has a control. In group column, each control is named by antibody. In cotrol column, cutandrun samples cut and run by antibody lists their antibodies. Control samples in control column list empty. So, in control column in the above samplesheet.csv, FOXA1 is for S90_R2_001.fastq.gz and empty is for S89_R2_001.fastq.gz. The second column is replicate, if an sample has multiple replicates, then arabic numbers are given for these replicates, for example, we suppose that three replicates are designed for an antiboy, "1" is specified to the first replicate,"2" to the second one, and 3 to the third one. The columns 3 and 4 are fastq_1,fastq_2 for paired-end fastqs of DNA-seq. One must give physical path for each fastq. For single-end fastq, then fastq_2 is given to empty. The following table describes each column in the samplesheet.csv
|column |Description                                |
|-------|-------------------------------------------|
|group  |Group identifier for sample. This will be identical for replicate samples from the same experimental group.|
|replicate|Integer representing replicate number. |
|fastq_1| Full path to FastQ file for read 1. File has to be zipped and have the extension ".fastq.gz" or ".fq.gz".|
|fastq_2|Full path to FastQ file for read 2. File has to be zipped and have the extension ".fastq.gz" or ".fq.gz".|
|control|String representing the control group in the group column to which this replicate is assigned to.|


One can use bash list to build samplesheet.csv under data folder:
```
(base) [yxt477@scccbbsr data]$ ls > samplesheet.csv 
```
Use vim to add physical path (/data1/Yuande/DNA_cutandrun/data2/) to each fastq data.

## Build nextflow commandline
Use vim to build nextflow commandline to perform cutandrun pipeline. Here is bash file  "run_nextflow_DNA_cutandrun2.sh" to run nf-core/cutandrun where the commandline is
```
#!/bin/bash
cd /data1/Yuande/DNA_cutandrun/

nextflow run nf-core/cutandrun -r 3.1 --input sample_infor3.csv --peakcaller 'seacr,MACS2' --genome GRCh38 --outdir result3 -profile docker
```
In this bash file, we use docker for saving image files and log files. The flog is "-profile". In our Linux server system, docker was installed. In pegasus (PHC) system, docker is not permised but one can use singularity to replace docker. In nf-core there are old version cutandrun 3.1 and the last version. The last cutandrun version is not available, so we use 3.1 version. Put samplesheet.csv after --input. Use seacr, MACS2 to do peak calling. Use GRCh38 as reference genome. Set outdir aname as result3 or nexflow_result under specified director folder.
## To run nextflow commandline
Give bash file a permission by chemod:
```
$ chemod 755 run_nextflow_DNA_cutandrun2.sh
```
Then run run_nextflow_DNA_cutandrun2.sh under current director folder:
```
./run_nextflow_DNA_cutandrun2.sh
```
## Output files
Cutandrun would output 5 director folders: 

01_prealign  

02_alignment  

03_peak_calling  

04_reporting  

pipeline_info

01_prealign has two subfolders: pretrim_fastqc and trimgalore. All fastqc.zip files are saved in pretrim_fastqc. consensus_upset_plots,  deeptools_heatmaps,  deeptools_qc,  igv,  multiqc, and  preseq are saved in 04_reporting. One can review multiqc_report.html in  multiqc folder. The multiqc_report.html shows all fastqc plots online. ComputeMatrix.vals.mat.tab,  lotHeatmap.mat.tab, and  plotHeatmap.pdf files can be found in 04_reporting/deeptools_heatmaps/peaks. In 03_peak_calling folder, you can find 01_bam_to_bedgraph  02_clip_bed,  03_bed_to_bigwig,  04_called_peaks,  05_consensus_peaks,  06_fragments_from_bams subfolders. 

