# 📝 Nextflow on your local

## Requirements

Nextflow can be used on any POSIX compatible system (Linux, OS X, etc). It requires Bash 3.2 (or later) and [Java 11 (or later, up to 20)](http://www.oracle.com/technetwork/java/javase/downloads/index.html) to be installed.

## Installation

Nextflow is distributed as a self-installing package, which means that it does not require any special installation procedure.

It only needs two easy steps:

1. Download the executable package by copying and pasting either one of the following commands in your terminal window: `wget -qO- https://get.nextflow.io | bash`

&#x20;      Or, if you prefer `curl`: `curl -s https://get.nextflow.io | bash`

&#x20;      This will create the `nextflow` main executable file in the current directory

2. Make the binary executable on your system by running `chmod +x nextflow`.
3. Optionally, move the `nextflow` file to a directory accessible by your `$PATH` variable (this is only required to avoid remembering and typing the full path to `nextflow` each time you need to run it).

{% hint style="info" %}
Set `export CAPSULE_LOG=none` to make the dependency installation logs less verbose.
{% endhint %}

{% hint style="info" %}
If you don't have `curl` or `wget`, you can also download the Nextflow launcher script from the [project releases page](https://github.com/nextflow-io/nextflow/releases/latest) on GitHub, in lieu of step 1.
{% endhint %}

{% hint style="info" %}
To avoid downloading the dependencies, you can also use the `nextflow-VERSION-all` distribution available for every Nextflow release on Github.

1. Go to the [Github releases page](https://github.com/nextflow-io/nextflow/releases) and expand the `Assets` section for a specific release.
2. Copy the URL of the `nextflow-VERSION-all` asset and enter the download command in your terminal, e.g. `wget -qO- ASSET-URL`. It will create the completely self-contained `nextflow-VERSION-all` executable file in the current directory.
{% endhint %}

## Updates

Having Nextflow installed in your computer you can update to the latest version using the following command:

```bash
nextflow self-update
```

{% hint style="info" %}
You can temporarily switch to a specific version of Nextflow by prefixing the `nextflow` command with the `NXF_VER` environment variable. For example:

```
NXF_VER=20.04.0 nextflow run hello
```
{% endhint %}
