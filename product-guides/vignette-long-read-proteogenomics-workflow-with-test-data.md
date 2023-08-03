# Vignette Long Read Proteogenomics Workflow with Test Data

### Setting up

Here, we repeat the steps found in README at the top of this repository, but we will work with all the processes found in this repository and describe the input files, the processing results and the output produced in these steps, so that the visitor may reproduce and see themselves. Throughout - emphasis on what specific files are required to enable a visitor to use this repository to analyze their own results.

Please note that machines need to be sized to match the size and number of samples to be processed. This vignette uses an artificially small sample, in general limited to one chromosome (Chr 22) and one mass spec file, but it is to illustrate the inputs, processing and output that is performed by this Nextflow workflow.

This quick start and steps were performed on a MacBook Pro running BigSur Version 11.4 with 16 GB 2667 MHz DDR48 RAM and a 2.3 GHz 8-Core Intel Core i9 processor.

The test and the data in the [![DOI](https://camo.githubusercontent.com/13f3fa0b97b78a1e2dd7c11e2d4fef1553bb497659496202b17b27c56b8df2b4/68747470733a2f2f7a656e6f646f2e6f72672f62616467652f444f492f31302e353238312f7a656e6f646f2e353233343635312e737667)](https://doi.org/10.5281/zenodo.5234651) completed as follows:

```
Duration    : 39m 40s
CPU hours   : 1.4
Succeeded   : 37
```

The visitor will be walked through the pre-requisites, clone the library and execute with demonstration data also used in the [`GitHub Actions`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/actions/workflows/ci\_test\_with\_sqanti.yml).

#### Obtain the Desktop DockerHub Application

![](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/raw/main/docs/images/Moby-logo.png)

In this quick start, [`Dockerhub Desktop Application for the Mac with an Intel Chip`](https://hub.docker.com/editions/community/docker-ce-desktop-mac) was used. Follow the instructions there to install.

#### Configure the Desktop DockerHub Application

On the MacBook Pro running BigSur Version 11.4 with 16 GB RAM, It was necessary to configure the Dockerhub resources to use **`6GB`** of RAM.

![](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/raw/main/docs/images/DockerHubDesktopResourceConfigurationMacWithIntelChip.png)

**Obtain miniconda**

On the MacBook Pro, the [`64-bit version of miniconda was downloaded and installed`](https://docs.conda.io/en/latest/miniconda.html) follow the installation instructions.

#### Create and activate a new conda environment `lrp`.

```bash
conda create -n lrp
conda activate lrp
```

#### Install Nextflow.

Install and set the Nextflow version.

```bash
conda install -c bioconda nextflow -y
export NXF_VER=20.01.0
```

#### Clone this repository

Now with the environment ready, we can clone.

```bash
git clone https://
.com/sheynkman-lab/Long-Read-Proteogenomics

cd Long-Read-Proteogenomics
```

### Execute

The input parameters are specified within this configuration file: [test with sqanti configuration](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/main/conf/test\_with\_sqanti.config). Inspecting this file we see reduced file sizes which serve dual purpose of this vignette as well as GitHub actions. The GitHub actions permit the automatic testing of the processes containerized and used with the Nextflow workflow within this repository.

This configuration file may be run directly from the repository:

```bash
nextflow run sheynkman-lab/Long-Read-Proteogenomics --config conf/test_with_sqanti.config
```

or if the user has cloned this repository, it may be run from the command line

```bash
nextflow run main.nf --config conf/test_with_sqanti.config 
```

This execution, runs all the steps, and unlike the quick start, does execute the `Sqanti3` which has special additional annotation features regarding the annotation of the protein and will be discussed within this vignette.

### Nextflow PublishDir

In the Nextflow setup in the `Main arguments` portion of the configuration file, the `name` is for the `PublishDir` output directory

```bash
   name                      = 'jurkat_chr22'
```

A Nextflow process has the pattern: input, process, output -- the process is a script. Additionally, there is an option to publish the results -- where these results go is setup as a relative directory defined by the user - in our case, the results of each of the processes by is defined by the `params.outdir/params.name` as well a further subdirectory. Keeping the relative path information easy for the user to follow and understand.

Using the `tree` command on the MacOS, install using `brew install tree` from a terminal window, need [`Homebrew`](https://treehouse.github.io/installation-guides/mac/homebrew)

### Results

We see the first level down two directories, `results` and `pipeline_info`.

```
results
├── jurkat_chr22
└── pipeline_info
```

### Results/jurkat\_chr22

In our case the output directory is called `results` and the `name` provided in the configuration file is `jurkat_chr22`.

Using the name specified in the main arguments, the output of each of the tasks/processes executed by Nextflow, produced by the scripts and published to these subdirectories using the `PublishDir` command within the process definition.

```
results/jurkat_chr22/
├── accession_mapping
├── cpat
├── gencode_db
├── hybrid_protein_database
├── isoseq3
├── metamorpheus
├── novel_peptides
├── orf_calling
├── pacbio_6frm_gene_grouped
├── pacbio_cds
├── peptide_analysis
├── protein_classification
├── protein_filter
├── protein_gene_rename
├── protein_group_compare
├── raw_convert
├── reference_tables
├── refined_database
├── rename_cds
├── sqanti3
├── sqanti3-filtered
├── sqanti_protein
├── star
├── track_visualization
└── transcriptome_summary
```

### Pipeline Information

The `pipeline_info` directory contains Nextflow generated files that are helpful in sizing, running and determining scaling options on this workflow.

```
results/pipeline_info/
├── execution_report.html
├── execution_timeline.html
├── execution_trace.txt
├── pipeline_dag.svg
└── test_with_sqanti_pipeline_dag.svg
```

We can inspect the `execution_report.html` downloading it as it was run with this configuration file previously by downloading it from Zenodo and opening it up in a browser.

```bash
wget https://zenodo.org/record/5234651/files/execution_report.html
```

Nextflow generates this report and inspecting it in a browser reveals many runtime details useful for review of the execution and planning for future executions. It contains tab selectable Summary, Resources and Tasks. Tasks show information about each task in the workflow. You can search for specific values, the filters include Metrics, Metadata and All. You can look at each of the tasks all together. All details are contained, including which container was used for each process, status for the run and a has to guide the user to the work directory under which the task was run, useful for debugging.

Inspecting the `execution_timeline.html`, also downloading it from Zenodo:

```bash
wget https://zenodo.org/record/5234651/files/execution_timeline.html
```

Opening this file in the browser we can see each of the processes as they executed and how long they took. Very useful for understanding what occurs when and how much CPU time the processes took.

### Workflow Overview

![Long Read Proteomics Workflow](https://user-images.githubusercontent.com/12956799/110397183-5c041b00-803f-11eb-9ba7-02352dab5656.png)

This workflow uses `Established Tools` and `Custom Tools` to bring together `Transcriptomic`, `Proteogenomic` and `Proteomic` information to make sample-specific annotated databases, permitting analysis and visualization of sample specific results.

Before we go through the `Transcriptomic`, `Proteogenomic` and `Proteomic` processes, we divert into providing a little background regarding our input data and Nextflow workflow channels.

#### Input Data

The `Input data` portion of the configuration file [test with sqanti configuration](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/main/conf/test\_with\_sqanti.config), we have the following parameters defined:

```
// Input data
   gencode_gtf               = 'https://zenodo.org/record/5234651/files/gencode.v35.annotation.chr22.gtf'
   gencode_transcript_fasta  = 'https://zenodo.org/record/5234651/files/gencode.v35.pc_transcripts.chr22.fa'
   gencode_translation_fasta = 'https://zenodo.org/record/5234651/files/gencode_protein.chr22.fasta'
   genome_fasta              = 'https://zenodo.org/record/5234651/files/GRCh38.primary_assembly.genome.chr22.fa'
   hexamer                   = 'https://zenodo.org/record/5234651/files/Human_Hexamer.tsv'
   logit_model               = 'https://zenodo.org/record/5234651/files/Human_logitModel.RData.gz'
   normalized_ribo_kallisto  = 'https://zenodo.org/record/5234651/files/kallist_table_rdeplete_jurkat.tsv'
   primers_fasta             = 'https://zenodo.org/record/5234651/files/NEB_primers.fasta'
   sample_ccs                = 'https://zenodo.org/record/5234651/files/jurkat.codethon_toy.ccs.bam'
   sample_kallisto_tpm       = 'https://zenodo.org/record/5234651/files/jurkat_gene_kallisto.tsv'
   star_genome_dir           = 'https://zenodo.org/record/5234651/files/star_genome.tar.gz'
   uniprot_protein_fasta     = 'https://zenodo.org/record/5234651/files/uniprot_protein.chr22.fasta'
```

The [`gencode_gtf`](https://zenodo.org/record/5234651/files/gencode.v35.annotation.chr22.gtf), [`gencode_transcript_fasta`](https://zenodo.org/record/5234651/files/gencode.v35.pc\_transcripts.chr22.fa), [`gencode_translation_fasta`](https://zenodo.org/record/5234651/files/gencode\_protein.chr22.fasta) and [`genome_fasta`](https://zenodo.org/record/5234651/files/GRCh38.primary\_assembly.genome.chr22.fa) have all been reduced to just `chromosome 22` of the human genome. Users can use the version of Gencode and compatible assembly of their genome of interest. Further details of the input parameters are found [here](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/wiki/Input-Parameters).

These data then are mapped in the course of the execution of the Nextflow workflow to [`channels`](https://www.nextflow.io/docs/latest/channel.html). Channels are used by Nextflow to enable parallel processing.

#### Nextflow channels

There are two types of Nextflow channels:

1. [`queue channels`](https://www.nextflow.io/docs/latest/channel.html#queue-channel) and
2. [`value channels`](https://www.nextflow.io/docs/latest/channel.html#value-channel).

A [`queue channel`](https://www.nextflow.io/docs/latest/channel.html#queue-channel) cannot be used more than one time as a process input or as a process output.

[`gencode_gtf`](https://zenodo.org/record/5234651/files/gencode.v35.annotation.chr22.gtf) gets mapped to the \[`value channel`]\([https://www.nextflow.io/docs/latest/channel.html#value-channel](https://www.nextflow.io/docs/latest/channel.html#value-channel) [`ch_gencode_gtf`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L73-L74).

```
ch_gencode_gtf = Channel.value(file(params.gencode_gtf))
```

[`gencode_transcript_fasta`](https://zenodo.org/record/5234651/files/gencode.v35.pc\_transcripts.chr22.fa) is mapped to either the channel `ch_gencode_transcript_fasta` or the \`ch\_gencode\_transcript\_fasta\_uncompressed channel depending upon whether or not it is compressed.

```python
if (params.gencode_transcript_fasta.endsWith('.gz')){
   ch_gencode_transcript_fasta = Channel.value(file(params.gencode_transcript_fasta))
} else {
   ch_gencode_transcript_fasta_uncompressed = Channel.value(file(params.gencode_transcript_fasta))
}
```

This permits the handling a compressed (filename ends with `gz`) or not. Regardless, note that [`gencode_transcript_fasta`](https://zenodo.org/record/5234651/files/gencode.v35.pc\_transcripts.chr22.fa) gets mapped to a [`value channel`](https://www.nextflow.io/docs/latest/channel.html#value-channel) can be read unlimited times without consuming its content.

There are several processes that offer file decompression, you may read about them in the `Utility Processes` section. Now we begin with `Transcriptomic Processes` description, then follow with `Proteogenomic Processes`, `Protein Processes` and finish with `Utility Processes`.

### Transcriptomic Processes

Beginning with [`Circular Consensus Reads`](https://www.nature.com/articles/s41587-019-0217-9), the IsoSeq3 process is executed to produced polished transcripts which are input to Sqanti3 for Isoform and Junction Annotations and Isoform Structures, CPAT which produces the beginning input required for the Proteogenomic processes of ORF Predictions, ORF calls and Database annotations, and the 6-Frame translation databases.

#### IsoSeq3 process

[`IsoSeq3 process`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L435-L491) uses as input `ch_sample_ccs`, `ch_genome_fasta_isoseq` and `ch_primers_fasta` files and produces in the `results/jurkat_chr22/isoseq3` directory.

```
results/jurkat_chr22/isoseq3/
├── jurkat_chr22.collapsed.abundance.txt
├── jurkat_chr22.collapsed.fasta
├── jurkat_chr22.collapsed.gff
├── jurkat_chr22.collapsed.report.json
├── jurkat_chr22.demult.lima.summary
├── jurkat_chr22.flnc.bam
├── jurkat_chr22.flnc.bam.pbi
└── jurkat_chr22.flnc.filter_summary.json
```

Only two outputs are mapped to queue channels for subsequent processing by other processes:

* `ch_isoseq_gtf` mapped from `jurkat_chr22.collapsed_gff` and
* `ch_fl_count` mapped from `jurkat_ch22.collapsed_abundance.txt`.

#### Sqanti3

[`sqanti3`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L591-L644): sqanti3 classifies IsoSeq3 isoforms and corrects isoforms using the star junction details relative to the reference genome if short read fastq files are provided.

```
results/jurkat_chr22/sqanti3
├── jurkat_chr22.params.txt
├── jurkat_chr22_classification.txt
├── jurkat_chr22_corrected.fasta
├── jurkat_chr22_corrected.gtf
├── jurkat_chr22_junctions.txt
└── jurkat_chr22_sqanti_report.pdf
```

Outputs mapped to queue channels for subsequent processing by other processes are:

* `ch_sample_unfiltered_classification` from `jurkat_chr22_classification.txt`,
* `ch_sample_unfiltered_fasta` from `jurkat_chr22_corrected.fasta`
* `ch_sample_unfiltered_gtf` from `jurkat_chr22_corrected_gtf`

#### CPAT

[`CPAT`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L810-L857) is a bioinformatics tool to predict an RNA’s coding probability based on the RNA sequence characteristics. To achieve this goal, CPAT calculates scores of sequence-based features from a set of known protein-coding genes and background set of non-coding genes.

* ORF size
* ORF coverage
* Fickett score
* Hexamer usage bias

[CPAT](https://cpat.readthedocs.io/en/latest/) builds a logistic regression model using these 4 features as predictor variables and the “protein-coding status” as the response variable. After evaluating the performance and determining the probability cutoff, the model can be used to predict new RNA sequences.

Output produced is as follows:

```
results/jurkat_chr22/cpat/
├── CPAT_run_info.log
├── jurkat_chr22.ORF_prob.best.tsv
├── jurkat_chr22.ORF_prob.tsv
├── jurkat_chr22.ORF_seqs.fa
├── jurkat_chr22.no_ORF.txt
├── jurkat_chr22.r
├── jurkat_chr22_cpat.error
└── jurkat_chr22_cpat.output
```

Three of these are passed to downstream processes

* `ch_cpat_all_orfs` comes from `jurkat_chr22.ORF_prob.tsv`
* `ch_cpat_best_orf` comes from `jurkat_chr22.ORF_prob.best.tsv`
* `ch_cpat_protein_fasta` comes from `jurkat_chr22.ORF_seqs.fa`

#### Transcriptome Summary

[`transcriptome_summary`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L766-L800) compares the abundance (CPM) based upon long-read sequencing to the abundances (TPM) inferred from short-read sequencing as computed by Kallisto (analyzed outside of this pipeline), additionally produces a pacbio\_gene reference table.

```
results/jurkat_chr22/transcriptome_summary/
├── gene_level_tab.tsv
├── pb_gene.tsv
└── sqanti_isoform_info.tsv
```

The three outputs are passed to downstream processes as follows:

* `ch_gene_level` comes from `gene_level_tab.tsv`
* `ch_sqanti_isoform_info` comes from `sqanti_isoform_info.tsv`
* `ch_pb_gene` comes from `pb_gene.tsv`

### Proteogenomic Processes

#### six frame translation

[`six_frame_translation`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L734-L763) generates a fasta file of all possible protein sequences derivable from each PacBio transcript translating the fasta file in all six frames(3+, 3-). This is used to examine what peptides could theoretically match the peptides found via a mass spectrometry search against GENCODE.

Output is just one file

```
results/jurkat_chr22/pacbio_6frm_gene_grouped/
└── jurkat_chr22.6frame.fasta
```

this file is mapped to the following queue channel

* `ch_six_frame` from `jurkat_chr22.6frame.fasta`

#### ORF calling

[`orf_calling`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L869-L914) Selects the most plausible ORF from each PacBio flnc transcript, using the following information:

* comparison of ATG start in transcript to reference (GENCODE) - selecitng the ORF with ATG start matching the one in the reference if it exists.
* coding probability score from CPAT
* number of upstream ATGs for the condidate ORF - decreasing the score as the number of upstream ATGs increases using sigmoid function.

Additionally provides calling confidence of each ORF called

* Clear Best ORF : best score and fewest upstream ATGs for all called ORFs
* Plausible ORF : not clearly the best, but with a decent CPAT coding score (> 0.364)
* Low quality ORF : low CPAT coding score (< 0.364)

The output is a single file:

```
results/jurkat_chr22/orf_calling/
└── jurkat_chr22_best_orf.tsv
```

* `ch_best_orf` comes from `jurkat_chr22_best_orf.tsv`

In this case, this queue channel is split to be consumed by `4` downstream processes

```
ch_best_orf.into{
  ch_best_orf_refine
  ch_best_orf_cds
  ch_best_orf_sqanti_protein
  ch_best_orf_pclass
}
```

#### Refined DB Generation

[`refine_orf_database`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L923-L956)

* filters ORF database to include only accessions with a CPAT coding score above a threshold (default is 0.0),
* filters ORFS to only include ORFs that have a stop codon
* Collapses transcripts that produce the same protein into one entry, keeping a base accession (first alphanumeric).
* Abundances of transcripts (CPM) are also collapsed during this process.

Output is:

```
results/jurkat_chr22/refined_database/
├── jurkat_chr22_orf_refined.fasta
└── jurkat_chr22_orf_refined.tsv
```

These output are mapped to the following queue channels:

* `ch_refined_info` is mapped from `jurkat_chr22_orf_refined.fasta`
* `ch_sample_fasta_refine`

#### Rename CDS to Exon

[`rename_cds_to_exon`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1010-L1039)

This routine performs the following functions:

* Preprocessing step to SQANTI Protein
* CDS is renamed to exon and transcript stop and start
* locations are updated to reflect CDS start and stop

The output produced is:

```
results/jurkat_chr22/rename_cds/
├── gencode.cds_renamed_exon.gtf
├── gencode.transcript_exons_only.gtf
├── jurkat_chr22.cds_renamed_exon.gtf
└── jurkat_chr22.transcript_exons_only.gtf
```

These output files are mapped to the following queue channels:

* `ch_sample_cds_renamed` is mapped from `jurkat_chr22.cds_renamed_exon.gtf`
* `ch_sample_transcript_exon_only` is mapped from `jurkat_chr22.transcript_exons_only.gtf`
* `ch_ref_cds_renamed` is mapped from `gencode.cds_renamed_exon.gtf`
* `ch_ref_transcript_exon_only` is mapped from `gencode.transcript_exons_only.gtf`

#### SQANTI Protein

This routine [`sqanti_protein`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1041-L1066) classifies protein splice sites and calculates additional statistics for the start and stop of an ORF.

The output produced is captured in a single file:

```
results/jurkat_chr22/sqanti_protein/
└── jurkat_chr22.sqanti_protein_classification.tsv
```

This file is mapped to the following queue channel

* `ch_sqanti_protein_classification` is mapped from `jurkat_chr22.sqanti_protein_classification.tsv`

#### 5' UTR

The routine [`five_prime_utr`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1069-L1103) determines the 5' UTR status and is an intermediate step for protein classification. The 5' UTR determination status is necessary for the latter step of protein category classification.

The output produced is captured in a single file:

```
results/jurkat_chr22/5p_utr/
└── jurkat_chr22.sqanti_protein_classification_w_5utr_info.tsv
```

This file is mapped to the following queue channel

* `ch_sqanti_protein_classification_w_5utr` is mapped from `jurkat_chr22.sqanti_protein_classification_w_5utr_info.tsv`

#### Protein Classification

This routine classifies protein based upon splicing and start site. It is really one of the most important routines and many of the previous routines feed into this process.

[`protein_classification`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1104-L1145) has the following main classifications:

* `pFSM`: full-protein-match. This means that the protein fully matches a gencode protein
* `pISM`: incomplete-protein-match. This protein is partially matching the gencode protein and is considered an N- or C- terminus truncation artifact.
* `pNIC`: novel-in-catalog. This means that this protein is comprised of known N- terminus, known exons, and/or C-terminus in a new combination creating resulting from a not-yet-been-seen-before splicing event.
* `pNNC`: novel-not-in-catalog. This is a protein composed of a novel N-terminus, novel exon, and/or novel C-terminus and therefore has not contains not a recombination of known entity made from a novel splicing event, but contains novel ends of the exons in such a way as not been seen before therefore not found in the gencode database before.

These are the results as derived from the transcriptomic portion of the experimental data. As measured by long read RNA sequencing. Now we enter the proteomic portion of this proteogenomic pipeline where these events are or are not confirmed. It is important to note that these events are conformational because they are derived from separate experiments, separate library constructions.

The output of this routine is found in the `sqanti_protein` subdirectory of the `results/jurkat_chr22`:

```
results/jurkat_chr22/sqanti_protein/
└── jurkat_chr22.sqanti_protein_classification.tsv
└── jurkat_chr22.sqanti_genes.tsv
```

These files are then mapped to the following Nextflow queue channels.

* `ch_protein_classification_unfiltered` is mapped from the `jurkat_chr22.sqanti_protein_classification.tsv` file
* `ch_pr_genes` is mapped from the `jurkat_chr22.genes.tsv` file

#### Protein Filtering

This step uses the output of sqanti\_protein and filters out proteins that are not classified as pFSM, pNIC or pNNC. Removing then pISMs (either N-terminus or C-terminus truncations). And removing pNNC with junctions after the stop codon (default 2).

[`filter_protein`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1197-L1225)

The output is published to `results/jurkat_chr22/protein_filter` subdirectory:

```
results/jurkat_chr22/protein_filter/
├── jurkat_chr22.classification_filtered.tsv
├── jurkat_chr22.filtered_protein.fasta
└── jurkat_chr22_with_cds_filtered.gtf
```

These files are then mapped to the following Nextflow queue channels:

* `ch_filtered_protein_classification` is mapped from `jurkat_chr22.classification_filtered.tsv`
* `ch_filtered_protein_fasta` is mapped from `jurkat_chr22.filtered_protein.fasta`
* `ch_filtered_cds` is mapped from `jurkat_chr22_with_cds_filtered.gtf`

#### Protein Hybrid Database

This process makes a hybrid database that is composed of high-confidence PacBio proteins and Gencode proteins. High-confidence is defined as genes in which the PacBio sampling is adequate (average transcript length 1-4 kb and a total of 3 counts per million (CPM) - derived from Kallisto - per gene. The CPMs derived from the Kallisto program was done outside of this Nextflow workflow and is run using short read RNA-sequencing.

[`make_hybrid_database`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1237-L1276)

The output from this process is published to `results/jurkat_chr22/hybrid_protein_database`

```
results/jurkat_chr22/hybrid_protein_database/
├── jurkat_chr22_cds_high_confidence.gtf
├── jurkat_chr22_high_confidence_genes.tsv
├── jurkat_chr22_hybrid.fasta
└── jurkat_chr22_refined_high_confidence.tsv
```

The following Nextflow queue channels are used in downstream processes:

* `ch_high_confidence_cds` is mapped from the `jurkat_chr22_cds_high_confidence.gtf` file
* `ch_sample_hybrid_fasta` is mapped from the `jurkat_chr22_hybrid.fasta` file
* `ch_refined_into_high_conf` is mapped from the `jurkat_chr22_refined_high_confidence.tsv` file.

These data will be used in visualization of the results.

### Proteomic Processes

Mass spec results were obtained from instrumentation that measures properly prepared aliquot from the same sample that the long read Proteomics library was constructed. Often times the mass spec data needs to be converted into a format that can be used by the main proteomics tool used in this workflow, Metamorpheus. This is done by the following process:

[`mass_spec_raw_convert`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1290-L1309)

In our `toy` example, we used only one mass spec file so the output produced is a single file, if we had more mass spec files, there would be an output file for each of the provided raw files.

```
results/jurkat_chr22/raw_convert/
└── 120426_Jurkat_highLC_Frac28.mzML
```

The Nextflow queue channels mapped from this output (which can flex to be for as many files that match the provided pattern as possible) is as follows:

* `ch_mass_spec_converted` is mapped from any output file in the directory as indicated by the `file(*)` pattern.

It is important to note that the same is true regarding the input.

on the input, the pattern sought is for any file within the the directory. This is honestly a utility function, we could have described it in the utility processes section. Now we describe the heart of the `Proteomic Processes` section, where we see three different databases used, GENCODE, Uniprot and a PacBio Sample Specific refined Database. In the paper, and the user, can then compare the results.

#### Metamorpheus GENCODE

This routine runs the program Metamorpheus, which performs a mass spectrum search using a database derived from GENCODE data.

[`metamorpheus_with_gencode_database`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1329-L1363)

The results are published to the `results/jurkat_chr22/metamorpheus/gencode` subdirectory

```
results/jurkat_chr22/metamorpheus/gencode/
├── search_results
│   ├── Task\ Settings
│   │   └── Task1SearchTaskconfig.toml
│   ├── Task1SearchTask
│   │   ├── 120426_Jurkat_highLC_Frac28.mzID
│   │   ├── AllPSMs.psmtsv
│   │   ├── AllPSMs_FormattedForPercolator.tab
│   │   ├── AllPeptides.Gencode.psmtsv
│   │   ├── AllQuantifiedPeaks.tsv
│   │   ├── AllQuantifiedPeptides.tsv
│   │   ├── AllQuantifiedProteinGroups.Gencode.tsv
│   │   ├── model.zip
│   │   ├── prose.txt
│   │   └── results.txt
│   └── allResults.txt
└── toml
    ├── CalibrationTask.toml
    ├── GlycoSearchTask.toml
    ├── GptmdTask.toml
    ├── SearchTask.toml
    └── XLSearchTask.toml
```

The Nextflow queue channels mapped from the following files are used in subsequent processes:

* `ch_gencode_peptides` is mapped from the file `search_results/Task1SearchTask/AllPeptides.Gencode.psmtsv`
* `ch_gencode_protein_group` is mapped from the file `search_results/Task1SearchTask/AllQuantifiedProteinGroups.Gencode.tsv`

#### Metamorpheus UniProt

The Metamorpheus Uniprot process runs Metamorpheus mass spectrum search using a database derived from UniProt data.

[`metamorpheus_with_uniprot_database`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1365-L1397)

This routine publishes its results to `results/jurkat_chr22/metamorpheus/uniprot` subdirectory

```
results/jurkat_chr22/metamorpheus/uniprot/
├── search_results
│   ├── Task\ Settings
│   │   └── Task1SearchTaskconfig.toml
│   ├── Task1SearchTask
│   │   └── AllPeptides.UniProt.psmtsv
│   └── allResults.txt
└── toml
    ├── CalibrationTask.toml
    ├── GlycoSearchTask.toml
    ├── GptmdTask.toml
    ├── SearchTask.toml
    └── XLSearchTask.toml
```

The Nextflow queue channels mapped from the following files are used in subsequent processes:

* `ch_uniprot_peptides` is mapped from the file `search_results/Task1SearchTask/AllPeptides.UniProt.psmtsv`
* `ch_uniprot_protein_group` is mapped from the file `search_results/Task1SearchTask/AllQuantifiedProteinGroups.UniProt.tsv`

#### Metamorpheus with PacBio Sample Specific refined Database

This routine runs MetaMorpheus mass spectrum search with a PacBio refined database

[`metamorpheus_with_sample_specific_database_refined`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1400-L1433)

This routine publishes its results to the `results/jurkat_chr22/metamorpheus/pacbio/refined` subdirectory

```
results/jurkat_chr22/metamorpheus/pacbio/refined/
├── search_results
│   ├── Task\ Settings
│   │   └── Task1SearchTaskconfig.toml
│   ├── Task1SearchTask
│   │   ├── 120426_Jurkat_highLC_Frac28.mzID
│   │   ├── AllPSMs.psmtsv
│   │   ├── AllPSMs_FormattedForPercolator.tab
│   │   ├── AllPeptides.jurkat_chr22.refined.psmtsv
│   │   ├── AllQuantifiedPeaks.tsv
│   │   ├── AllQuantifiedPeptides.tsv
│   │   ├── AllQuantifiedProteinGroups.jurkat_chr22.refined.tsv
│   │   ├── model.zip
│   │   ├── prose.txt
│   │   └── results.txt
│   └── allResults.txt
└── toml
    ├── CalibrationTask.toml
    ├── GlycoSearchTask.toml
    ├── GptmdTask.toml
    ├── SearchTask.toml
    └── XLSearchTask.toml
```

Two Nextflow queue channels are defined and mapped from the following two files:

* `ch_pacbio_peptides_refined` is mapped from `search_results/Task1SearchTask/AllPeptides.jurkat_chr22.refined.psmtsv`
* `ch_pacbio_protein_groups_refined` is mapped from `"search_results/Task1SearchTask/AllQuantifiedProteinGroups.jurkat_chr22.refined.tsv`

#### MetaMorpheus with PacBio Filtered Sample Specific Database

This runs the MetaMorpheus mass spectrum search using the PacBio filtered database.

[`metamorpheus_with_sample_specific_database_filtered`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1440-L1473) publishes its results to results/jurkat\_chr22/metamorpheus/pacbio/filtered

```
results/jurkat_chr22/metamorpheus/pacbio/filtered/
├── search_results
│   ├── Task\ Settings
│   │   └── Task1SearchTaskconfig.toml
│   ├── Task1SearchTask
│   │   └── AllPeptides.jurkat_chr22.filtered.psmtsv
│   └── allResults.txt
└── toml
    ├── CalibrationTask.toml
    ├── GlycoSearchTask.toml
    ├── GptmdTask.toml
    ├── SearchTask.toml
    └── XLSearchTask.toml
```

The following Nextflow queue channels are mapped from the published output files as follows:

* `ch_pacbio_peptides_filtered` is mapped from the file `search_results/Task1SearchTask/AllPeptides.jurkat_chr22.filtered.psmtsv`
* `ch_pacbio_protein_groups_filtered` is mapped from the file `search_results/Task1SearchTask/AllQuantifiedProteinGroups.jurkat_chr22.filtered.tsv`

#### Metamorpheus with a Pacbio hybrid database

This run of MetaMorpheus mass spectrum search is against the hybrid database constructed for the PacBio transcripts.

[`metamorpheus_with_sample_specific_database_hybrid`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1480-L1516).

The results from this run are published in the `results/jurkat_chr22/metamorpheus/pacbio/hybrid` subdirectory

```
results/jurkat_chr22/metamorpheus/pacbio/hybrid/
├── search_results
│   ├── Task\ Settings
│   │   └── Task1SearchTaskconfig.toml
│   ├── Task1SearchTask
│   │   ├── 120426_Jurkat_highLC_Frac28.mzID
│   │   ├── AllPSMs.psmtsv
│   │   ├── AllPSMs_FormattedForPercolator.tab
│   │   ├── AllPeptides.jurkat_chr22.hybrid.psmtsv
│   │   ├── AllQuantifiedPeaks.tsv
│   │   ├── AllQuantifiedPeptides.tsv
│   │   ├── AllQuantifiedProteinGroups.jurkat_chr22.hybrid.tsv
│   │   ├── model.zip
│   │   ├── prose.txt
│   │   └── results.txt
│   └── allResults.txt
└── toml
    ├── CalibrationTask.toml
    ├── GlycoSearchTask.toml
    ├── GptmdTask.toml
    ├── SearchTask.toml
    └── XLSearchTask.toml
```

The following Nextflow queue channels are derived from the indicated published files as stated here:

* `ch_pacbio_peptides_hybrid` is mapped from `search_results/Task1SearchTask/AllPeptides.jurkat_chr22.hybrid.psmtsv`
* `ch_pacbio_protein_groups_hybrid` is mapped from `search_results/Task1SearchTask/AllQuantifiedProteinGroups.jurkat_chr22.hybrid.tsv`

#### MetaMorpheus using Rescue and Resolve Algorithm

This runs the MetaMorpheus mass spectrum search using the hybrid database and the version of MetaMorpheus that contains the "Rescue and Resolve" algorithm. The "Rescue and Resolve" algorithm allows for the "Rescue" of protein isoforms that are normally discarded during protein parsimony, given that the protein isoform has strong evidence from transcriptional expression.

[`metamorpheus with sample_specific_database_rescue_resolve`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1524-L1560).

The output is published in the `results/jurkat_chr22/metamorpheus/pacbio/rescue_resolve` subdirectory.

```
results/jurkat_chr22/metamorpheus/pacbio/rescue_resolve/
├── search_results
│   ├── Task\ Settings
│   │   └── Task1SearchTaskconfig.toml
│   ├── Task1SearchTask
│   │   ├── 120426_Jurkat_highLC_Frac28.mzID
│   │   ├── AllPSMs.psmtsv
│   │   ├── AllPSMs_FormattedForPercolator.tab
│   │   ├── AllPeptides.jurkat_chr22.rescue_resolve.psmtsv
│   │   ├── AllQuantifiedPeaks.tsv
│   │   ├── AllQuantifiedPeptides.tsv
│   │   ├── AllQuantifiedProteinGroups.jurkat_chr22.rescue_resolve.tsv
│   │   ├── model.zip
│   │   ├── prose.txt
│   │   └── results.txt
│   └── allResults.txt
└── toml
    ├── CalibrationTask.toml
    ├── GlycoSearchTask.toml
    ├── GptmdTask.toml
    ├── SearchTask.toml
    └── XLSearchTask.toml
```

There are no Nextflow Queue channels defined by the output files. We have ended the journey and have only visualization and peptide analysis remaining.

### Utility Processes

#### Decompression Processes

[`gunzip_gencode_transcript_fasta`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L256-L275): producing `ch_gencode_transcript_fasta_uncompressed`.

[`gunzip_logit_model`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L190-L209): produces `ch_logit_model_uncompressed`

[`untar_mass_spec`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L211-L233): produces two optional sets of file types \[`*.raw`] and \[`*.mzML| *.mzml`] mapped to channels `ch_mass_spec_raw and` ch\_mass\_spec\_mzml\` respectively.

[`gunzip_translation_fasta`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L235-L254): produces uncompressed files mapped to the `ch_gencode_translation_fasta_uncompressed` channel.

[`gunzip_genome_fasta`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L277-L296): produces the uncompressed files mapped to the `ch_genome_fasta_uncompressed` channel.

[`gunzip_uniprot_protein_fasta`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L298-L317): produces the uncompressed files mapped to the `ch_uniprot_protein_fasta_uncompressed` channel.

#### Generate Reference Tables

[`generate_reference_tables`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L323-L354): The ch\_gencode\_transcript\_fasta\_uncompressed and ch\_gencode\_gtf channels are used together only in this process. This setup process generates needed tabular data sets to be used in downstream processing including ensg\_gene.tsv, enst\_isoname.tsv, gene\_ensp.tsv, gene\_isoname.tsv, isoname\_lens.tsv, gene\_lens.tsv and protein\_coding\_genes.txt from gencode\_gtf and gencode\_transcript\_fasta.

This process creates several [`queue channels`](https://www.nextflow.io/docs/latest/channel.html#queue-channel). These are split as follows:

* [`ch_protein_coding_genes`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L356-L360) is split into \[`ch_protein_coding_genes_gencode_fasta`] to be consumed by process [`make_gencode_database`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L379-L415) and \[`ch_protein_coding_genes_filter_sqanti`] to be consumed by process [`filter_sqanti`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L655-L731).
* [`ch_ensg_gene`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L362-L366) is split into \[`ch_ensg_gene_filter`] to be consumed by process [`filter_sqanti`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L655-L731), \[`ch_ensg_gene_six_frame`] to be consumed by process [`six_frame_translation`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L734-L764) and \[`ch_ensg_gene_pclass`] to be consumed by process [`protein_classification`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1104-L1145).
* [`ch_gene_lens`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L367-L370) is split into \[`ch_gene_lens_transcriptome`] to be consumed by \[`...`], \[`ch_gene_lens_aggregate`] to be consumed by \[`...`].
* [`ch_gene_isoname`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L372-L375) is split into \[`ch_gene_isoname_pep_viz`] to be consumed by process \[`...`] and \[`ch_gene_isoname_pep_analysis`] to be consumed by process \[`...`].

#### Make\_Gencode\_Database

Using the output of the prepare\_reference\_tables channel, `ch_protein_coding_genes_gencode_fasta` and the `gencode_translation_fasta_uncompressed` are input to the

[`make_gencode_database`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L379-L407): The process clusters same-protein GENCODE entries and produces a non-redundant gencode\_protein.fasta file and gencode\_isoname\_clusters containing transcript accessions that were clustered. These are represented in one file that goes

#### Process\_generate\_star\_genome

[`process_generate_star_genome`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L517-L557): STAR Alignment run only if sqanti has not been previously been run and if fastq (short read RNAseq) files have been provided. STAR alignment is run if fastq reads are provided which results in sample junction alignments that are fed to SQANTI3 for classification filtering

#### Visualization Processes

[`gencode_track_visualization`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1598-L1624): Generates the tracks that can be used in the UCSC genome browser for the gencode database.

[`peptide_track_visualization`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/blob/23e345dafb0ef90e479cac94a29e3d702472e370/main.nf#L1736-L1836): Makes peptide tracks use in the UCSC Genome Browser for refined, filtered and hybrid databases

To visualize results, please see the [`visualization capabilities of the final annotated results`](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/wiki/Vignette---How-to-Visualize-Results-from-Jurkat-Analysis).

### Detailed View of Processing steps

There are `37` separate processes that are executed in this configuration file. Some specific to the running of this test vignette, such as the untarring of the compressed star\_genome directory. During the course of execution, Nextflow generates a diagram of the connectivity between channels and their processes as shown here.

![test\_with\_sqanti\_pipeline\_dag.svg](https://github.com/sheynkman-lab/Long-Read-Proteogenomics/raw/main/docs/images/test\_with\_sqanti\_pipeline\_dag.svg)

Though there are `37` processes defined in the `Nextflow` `main.nf` there are only `25` that produce output to this results directory.

This concludes the overview of running the `Long Read Proteogenomics Workflow with Test Data` vignette. If you have any questions, comments, etc. Please write us, make an issue and we will endeavour to answer as soon as possible.
