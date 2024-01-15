# CutandRun
 CUT&RUN-Flow (CnR-flow), a Nextflow pipeline for QC, tag trimming, normalization, and peak calling for paired-end sequencing data from CUT&RUN experiments. This software is available via GitHub, at http://www.github.com/RenneLab/CnR-flow.
 Full project documentation is available at CUT&RUN-Flow's https://cnr-flow.readthedocs.io/#
 
CUT&RUN-Flow was built by using Nextflow, a powerful domain-specific workflow language built to create flexible and efficient bioinformatics pipelines. Nextflow provides extensive flexibility in utilizing cluster computing environments such as PBS and SLURM, and in automated and compartmentalized handling of dependencies using Conda / Bioconda, Docker, Singularity or Environment Modules.

The pipeline is built using Nextflow, a workflow tool to run tasks across multiple compute infrastructures in a portable, reproducible manner. It is capable of using containerisation and package management making installation trivial and results highly reproducible. The Nextflow DSL2 implementation of this pipeline uses one container per process, which makes it easier to maintain and update software dependencies. Where possible, these processes have been submitted to and installed from nf-core/modules.

The pipeline has been developed with continuous integration (CI) and test driven development (TDD) at its core. nf-core code and module linting as well as a battery of over 100 unit and integration tests run on pull request to the main repository and on release of the pipeline. On official release, automated CI tests run the pipeline on a full-sized dataset on AWS cloud infrastructure. This ensures that the pipeline runs on AWS, has sensible resource allocation defaults set to run on real-world datasets, and permits the persistent storage of results to benchmark between pipeline releases and other analysis sources. The results obtained from the full-sized test can be viewed on the nf-core website.
 
## install nextflow with conda. 
1. create evironment with python 2.7
   
```
conda install -c bioconda nextflow
```

This installation is in base environment under your account. You may use conda create new environment and under the new environment you can install nextflow.

You don't install cutandrun pipeline.

After that, you need make an samplesheet.csv. Another attached file is a model for you to make the samplesheet.csv where the title must be group,replicate,fastq_1,fastq_2,control and data need physical paths. 

You also give output path in commandline.
