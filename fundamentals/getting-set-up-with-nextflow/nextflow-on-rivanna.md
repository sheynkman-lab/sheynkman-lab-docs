---
cover: ../../.gitbook/assets/nf.png
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🔄 Nextflow on Rivanna

Workflow managers are used to create reproducible and scalable analysis pipelines. These managers are useful when you have a series of scripts that you want to tie together in the form of a pipeline.

Nextflow enables scalable and reproducible scientific workflows using software containers.

## Available Versions <a href="#available-versions" id="available-versions"></a>

The current installation of Nextflow incorporates the most popular packages. To find the available versions and learn how to load them, run:

```
module spider nextflow
```

The output of the command shows the available Nextflow module versions.

For detailed information about a particular Nextflow module, including how to load the module, run the `module spider` command with the module’s full version label. **For example**:

```
module spider nextflow/20.10.0

```

<table><thead><tr><th width="203">Module</th><th width="171.33333333333331">Version</th><th>Module Load Command</th></tr></thead><tbody><tr><td>nextflow</td><td>20.10.0</td><td>module load nextflow/20.10.0</td></tr></tbody></table>

## Slurm for Snakemake: <a href="#slurm-for-snakemake" id="slurm-for-snakemake"></a>

* The Nextflow pipeline can be executed using a `SLURM` script on Rivanna
* Below is an example script to submit to the standard partition&#x20;

```sh
#!/bin/bash
#SBATCH --job-name="run_lrp_jurkat_test" 
#SBATCH --ntasks-per-node=32 
#SBATCH --cpus-per-task=1 #number of cores to use 
#SBATCH --time=1-12:00:00 #amount of time for the whole job 
#SBATCH --mem=500GB 
#SBATCH --partition=standard #the queue/partition to run on 
#SBATCH --account=sheynkman_lab 
#SBATCH --output=%x-%j.log 
#SBATCH --mail-type=BEGIN,END,FAIL 
#SBATCH --mail-user=ncp5qm@virginia.edu

module purge 
module load singularity nextflow/20.10.0

export SINGULARITY_CACHEDIR=/scratch/$USER/singularity_cache 
export NXF_SINGULARITY_CACHEDIR=$SINGULARITY_CACHEDIR

nextflow run sheynkman-lab/Long-Read-Proteogenomics -r v1.0.0 --config conf/test.config -with-singularity -without-docker ~
```
