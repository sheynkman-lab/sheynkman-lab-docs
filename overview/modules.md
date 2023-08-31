---
description: >-
  This page goes over important modules of the Long-Read Proteogenomics
  pipeline.
---

# ✨ Modules

## Gencode\_db

Create database from a curated reference source (GENCODE)

## IsoSeq3&#x20;

The [IsoSeq3 module](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/tree/main/modules/isoseq3) within our Nextflow pipeline harnesses the power of PacBio long-read sequencing for comprehensive transcript analysis. This approach offers a reference-independent perspective on transcriptomes, enabling the study of isoforms, alternative splicing, and novel genes. Here's a succinct SLURM script to execute the IsoSeq3 module:

```bash
#!/bin/bash
#SBATCH --job-name="run_isoseq3"
#SBATCH --nodes=1 #number of cores to use
#SBATCH --ntasks-per-node=32
#SBATCH -t 10:00:00 #amount of time for the whole job
#SBATCH -p standard #the queue/partition to run on
#SBATCH -A sheynkman_lab
#SBATCH --output=%x-%j.log
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=your_email@virginia.edu

module purge
module load singularity nextflow/20.10.0

export SINGULARITY_CACHEDIR=/scratch/$USER/singularity_cache  
export NXF_SINGULARITY_CACHEDIR=$SINGULARITY_CACHEDIR

ODIR=/path/to/isoseq/results
CCS_READS=/path/to/all_reads
BARCODES=/path/to/barcodes.fasta
GENOME_FASTA=/path/to/GRCh38.primary_assembly.genome.fa
NAME= project_name

NEXTFLOW_LOCATION=https://github.com/sheynkman-lab/IsoSeq-Nextflow

nextflow run $NEXTFLOW_LOCATION \
-r v1.0.0 \
--ccs_reads $CCS_READS \
--barcodes $BARCODES \
--genome_fasta $GENOME_FASTA \
--name $NAME \
--outdir $ODIR \
-with-singularity \
-without-docker 
```

## SQANTI3&#x20;

Integrates IsoSeq3 output for precise transcript curation. Key features:

* Annotation and quality control ensure reliable isoforms.
* GSTM predicts coding/non-coding transcripts.
* Prioritizes splice variations, aids expression analysis.

Unveils intricate transcriptome insights, advancing long-read analysis. SQANTI paper [here](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5848618/)

```bash
#!/bin/bash
#SBATCH --job-name="run_sqanti3"
#SBATCH --nodes=1 #number of cores to use
#SBATCH --ntasks-per-node=32
#SBATCH -t 10:00:00 #amount of time for the whole job
#SBATCH -p standard #the queue/partition to run on
#SBATCH -A sheynkman_lab
#SBATCH --output=%x-%j.log
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=your_email@virginia.edu

module purge
module load singularity nextflow/20.10.0

export SINGULARITY_CACHEDIR=/scratch/$USER/singularity_cache #this could also be in home or project storage 
export NXF_SINGULARITY_CACHEDIR=$SINGULARITY_CACHEDIR

NEXTFLOW_DIR=/path/to/Long-Read-Proteogenomics
REFERENCE_DIR=/path/to/GENCODEV38
GENOME_FASTA=/path/to/GRCh38.primary_assembly.genome.fa
GENCODE_GTF=/path/to/gencode.v38.basic.annotation.gtf
NAME=project_name

RESULTS_DIR=/path/to/isoseq/results
SAMPLE_GTF=/path/to/collapsed.gff
SAMPLE_GTF=/path/to/results/isoseq3/collapse/SMC.collapsed.gff
FL_COUNT=/path/to/isoseq3/collapse/SMC.mapped_fl_count.txt
OUTDIR=/path/to/sqanti/results

nextflow $NEXTFLOW_DIR/modules/sqanti3/sqanti3.nf \
--name          $NAME \
--genome_fasta  $GENOME_FASTA \
--gencode_gtf   $GENCODE_GTF \
--sample_gtf    $SAMPLE_GTF \
--fl_count      $FL_COUNT \
--outdir        $OUTDIR \
-with-singularity -without-docker
```

## CPAT and ORF calling

This module addresses ORF calling intricacies observed in SQANTI by employing CPAT. CPAT classifies transcripts as coding/non-coding, producing intelligible ORF candidate outputs. Many isoforms show frame-shifting splice errors. Thus, the 'orf\_calling' module extends CPAT, ranking ORFs for each transcript based on biological plausibility. Criteria include upstream ATG, abundance, and stop codon considerations.

```bash
#!/bin/bash
#SBATCH --job-name="run_cpat"
#SBATCH --ntasks-per-node=32
#SBATCH --cpus-per-task=1 #number of cores to use
#SBATCH --time=1-12:00:00 #amount of time for the whole job
#SBATCH --mem=500GB
#SBATCH --partition=standard #the queue/partition to run on
#SBATCH --account=sheynkman_lab
#SBATCH --output=%x-%j.log
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=your_email@virginia.edu

module purge
module load singularity nextflow/20.10.0

export SINGULARITY_CACHEDIR=/scratch/$USER/singularity_cache 
export NXF_SINGULARITY_CACHEDIR=$SINGULARITY_CACHEDIR

NAME=project_name
INPUT_FASTA=/path/to/results/sqanti3/SMC_corrected.fasta 
INPUT_HEXAMER=/path/to/Human_Hexamer.tsv 
INPUT_MODEL=/path/to/Human_logitModel.RData 
MIN_ORF=50
CPAT_NXF_DIR=/path/to/cpat.nf
NXF_CONF_DIR=/path/to/nextflow.config
OUTDIR=/path/to/cpat/output/

nextflow run $CPAT_NXF_DIR --config $NXF_CONF_DIR \
--name $NAME \
--hexamer $INPUT_HEXAMER \
--logit_model $INPUT_MODEL \
--sample_fasta $INPUT_FASTA \
--min_orf $MIN_ORF \
--outdir $OUTDIR \
-with-singularity -without-docker
```

## SQANTI Protein

Looked at every variation of a protein isoform&#x20;

\+ ' protein\_classification' - each PacBio transcript, we classify the protein for the plausible biological orf

But we find transcripts of same gene can produce same ORFs. Some produce small ORFs hence may not be relevant.&#x20;

## More helper modules

## Track visualization
