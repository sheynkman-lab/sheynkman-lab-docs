---
description: These instructions will be followed for the LRP Bootcamp
---

# Custom LRP run

## Download  data <a href="#step-01-download-gencode-data" id="step-01-download-gencode-data"></a>

Download necessary input data for your custom LRP run.

{% hint style="info" %}
For the purpose of this LRP bootcamp, test data from Jurkat sample will be used. The Jurkat sample data (chr 22) can be found on [Zenodo](https://zenodo.org/record/5234651), but you don't have to download any file as the Zenodo URLs are alreay linked with the .config file.
{% endhint %}

***

## Clone LRP pipeline from GitHub <a href="#step-02-get-lrp-pipeline-from-github" id="step-02-get-lrp-pipeline-from-github"></a>

```bash
git clone git@github.com:sheynkman-lab/Long-Read-Proteogenomics.git
```

* The example code provided above assumes GitHub is set-up to use SSH keys for authentication.&#x20;
* The key can be generated with the `ssh-keygen` command on the terminal if not already set up. For a detailed step-by-step guide follow [this guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).
* Alternatively, `https` url of the repository can be used. Please make sure to configure git by using `git config --global` options to avoid authentication issues.

***

## Prepare LRP nextflow .config file <a href="#step-03-prepare-lrp-config-file" id="step-03-prepare-lrp-config-file"></a>

* The previous step downloads the [`Long-Read-Proteogenomics`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics) folder in the current working directory.
*   Create a new configuration file `<Run_name>.config` (i.e. `test_run_lrp.config`)

    ```bash
    vim test_run_lrp.config
    ```
* Follow the provided template [`Long-Read-Proteogenomics/conf/test.config`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/main/conf/test.config) to populate the newly created `test_run_lrp.config` file.
* Make sure to update the `outdir` parameter providing path to output (i.e. `/project/sheynkman/users/xyz/project...`)
* For more details on parameters see LRP [wiki](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/wiki).

{% hint style="info" %}
`The`[`Long-Read-Proteogenomics/conf/test_without_sqanti.config`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/main/conf/test\_without\_sqanti.config)will be used for this LRP bootcamp.

Note: Before you proceed, please change the number of `max_cpus to 1`.&#x20;
{% endhint %}

***

## Prepare and run SLURM job to run the .config file <a href="#step-04-prepare-slurm-job" id="step-04-prepare-slurm-job"></a>

The overall run can be performed using the following commands. Here we wrap the following code in a slurm job script that can be executed on Rivanna

Below is a slurm script that you can use to test the LRP pipeline using the Jurkat test data.&#x20;

Note: This slurm script should be inside the `Long-Read-Proteogenomics/` folder

```sh
#!/bin/bash
#SBATCH --job-name="run_lrp_jurkat_test"
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=2 #number of cores to use
#SBATCH --time=1:00:00 #amount of time for the whole job
#SBATCH --mem=246GB
#SBATCH --partition=dev #the queue/partition to run on
#SBATCH --account=sheynkman_lab
#SBATCH --output=%x-%j.log
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=your_id@virginia.edu

module purge
module load singularity nextflow/20.10.0

export SINGULARITY_CACHEDIR=/scratch/$USER/singularity_cache 
export NXF_SINGULARITY_CACHEDIR=$SINGULARITY_CACHEDIR 

nextflow run main.nf --config conf/test_without_sqanti.config -with-singularity -without-docker

```

{% hint style="info" %}
For this LRP test run, please run the commands on your interactive mode or command line (recommended). For this purpose, the #SBATCH commands will not be required.
{% endhint %}

Add job configurations with `SBATCH parameters`. For more details on job scripts refer to this [documentation](https://www.rc.virginia.edu/userinfo/rivanna/slurm/) provided by the UVA Research Compute.

```bash
sbatch EC_LRP_03_23_2023.slurm
```

* The above command returns a job id (e.g. `7381199`)
*   This id can be used to track the job status

    ```bash
    scontrol show job 7381199
    ```
