---
description: 'sheynkman-lab/Long-Read-Proteogenomics: Usage'
coverY: 75
---

# 📪 Usage

## Running the pipeline

The typical command for running the pipeline is as follows:

<pre class="language-bash"><code class="lang-bash"><a data-footnote-ref href="#user-content-fn-1">nextflow run sheynkman-lab/Long-Read-Proteogenomics --input '*_R{1,2}.fastq.gz' -profile docker</a>
</code></pre>

This will launch the pipeline with the `docker` configuration profile. See below for more information about profiles.\
\
Note that the pipeline will create the following files in your working directory:

```
work            # Directory containing the nextflow working files
results         # Finished results (configurable, see below)
.nextflow_log   # Log file from Nextflow
# Other nextflow hidden files, eg. history of pipeline runs and old logs.

```

## Updating the pipeline

When you run the above command, Nextflow automatically pulls the pipeline code from GitHub and stores it as a cached version. When running the pipeline after this, it will always use the cached version if available - even if the pipeline has been updated since. To make sure that you're running the latest version of the pipeline, make sure that you regularly update the cached version of the pipeline

```bash
nextflow pull sheynkman-lab/Long-Read-Proteogenomics
```

## Reproducibility

It's a good idea to specify a pipeline version when running the pipeline on your data. This ensures that a specific version of the pipeline code and software are used when you run your pipeline. If you keep using the same tag, you'll be running the same version of the pipeline, even if there have been changes to the code since.

First, go to the [sheynkman-lab/Long-Read-Proteogenomics releases page](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/releases) and find the latest version number - numeric only (eg. `1.3.1`). Then specify this when running the pipeline with `-r` (one hyphen) - eg. `-r 1.3.1`.

This version number will be logged in reports when you run the pipeline, so that you'll know what you used when you look back in the future.

[^1]: 
