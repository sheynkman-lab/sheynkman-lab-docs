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

# 🛠 Getting set up with Nextflow

In life science research, a gap often exists between bioinformaticians who develop analysis scripts, and biologists who run them. Biologists struggle to understand and adapt code to their needs, while coders lack the biological insight to make scripts fit research objectives.

Workflow management tools bridge this gap. They allow biologists to chain command line tasks into automated pipelines for data analysis, without needing to code themselves. In this bootcamp you will learn essential skills to build robust workflows for genomics, proteomics, metabolomics and more using tools like Snakemake and Nextflow. By the end, you will be able to reproduce analyses without coding expertise.

Main points:

* Paragraph 1: Explains the gap between scripters (coders) and biologists, and the challenges it creates.
* Paragraph 2: Introduces workflow tools as the solution, and outlines the bootcamp objectives and benefits.

## Terminology: What is it?

Nextflow is a workflow management system, it defines the input and output path, and puts executable scripts (mainly linux commands) into triple quotes.

It has an abstraction layer so it is portable to cloud, docker and other HPC environments.

<figure><img src="../../.gitbook/assets/nextflow-k8s-min (1).png" alt=""><figcaption></figcaption></figure>

Nextflow is a workflow management tool that automates the execution of computational pipelines for research. It allows researchers to define sequences of data analysis operations that will run automatically. A key benefit of Nextflow is portability - it abstracts away the underlying execution environment through its executor system. This means Nextflow pipelines can seamlessly run on a laptop, the cloud, clusters, or other environments just by changing the executor configuration. Researchers don't have to rewrite code to port analyses. Nextflow handles executing pipeline steps across different platforms, enabling portable and replicable analysis workflows. With its simple abstraction layer, Nextflow allows researchers to focus on data analysis logic rather than infrastructure. Pipelines defined in Nextflow are resilient to changes in computing environment due to this executor portability.
