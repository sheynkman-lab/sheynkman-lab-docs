---
description: >-
  This page goes over important modules of the Long-Read Proteogenomics
  pipeline.
---

# ✨ LRP Modules

## Gencode\_db

Create database from a curated reference source (GENCODE)

## IsoSeq3&#x20;

Example slurm script to run [IsoSeq3 module](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/tree/main/modules/isoseq3).&#x20;

Transcript sequences and abudances derived froim PacBio sequencing. This is reference independant.

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

## SQANTI3

Uses the IsoSeq3 output to curation of long-read transcriptomes for accurate identification of known and novel isoforms.&#x20;

Annotation + quality control: paper [here](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5848618/)

GSTM - ORF prediction within SQANTI3 for coding and non-coding

The genetic variants are masked in this version of pipeline and only splice variations are kept.

Interested in diff gene and isoform expression : parameteres of interest in SQANTI.

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

The embedded ORF called in SQANTI is rusty. Hence CPAT used and it classifies if a transcript is coding or non-coding. The output of CPAT is very understandabel and readable.  - ORF candidates.

We notice that a lot of transcript isoforms have splice errors that shift the translating frame.  Hence we include 'orf\_calling' module to CPAT to be able to rank the ORF for each transcript and determine which one is the most biologically plaussible : upstream ATG ..  care abput abundance and stop codon.&#x20;

## SQANTI Protein

Looked at every variation of a protein isoform&#x20;

\+ ' protein\_classification' - each PacBio transcript, we classify the protein for the plausible biological orf

But we find transcripts of same gene can produce same ORFs. Some produce small ORFs hence may not be relevant.&#x20;

## More helper modules

## Track visualization
