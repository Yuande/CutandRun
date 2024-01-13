# CutandRun
 CUT&RUN-Flow (CnR-flow), a Nextflow pipeline for QC, tag trimming, normalization, and peak calling for paired-end sequencing data from CUT&RUN experiments. This software is available via GitHub, at http://www.github.com/RenneLab/CnR-flow.
 Full project documentation is available at CUT&RUN-Flow's https://cnr-flow.readthedocs.io/#
 
CUT&RUN-Flow was built by using Nextflow, a powerful domain-specific workflow language built to create flexible and efficient bioinformatics pipelines. Nextflow provides extensive flexibility in utilizing cluster computing environments such as PBS and SLURM, and in automated and compartmentalized handling of dependencies using Conda / Bioconda, Docker, Singularity or Environment Modules.
 
## install nextflow with conda. 
1. create evironment with python 2.7
   
```
conda install -c bioconda nextflow
```

This installation is in base environment under your account. You may use conda create new environment and under the new environment you can install nextflow.

You don't install cutandrun pipeline.

After that, you need make an samplesheet.csv. Another attached file is a model for you to make the samplesheet.csv where the title must be group,replicate,fastq_1,fastq_2,control and data need physical paths. 

You also give output path in commandline.
