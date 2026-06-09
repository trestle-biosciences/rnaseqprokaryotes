# trestlebiosciences/rnaseqprokaryotes

## Introduction

**trestlebiosciences/rnaseqprokaryotes** is a bioinformatics pipeline that performs read quality control, adapter/quality trimming, reference indexing and alignment, annotation conversion, feature quantification, and run-level reporting for prokaryotic RNA-seq data.


<!-- TODO nf-core: Include a figure that guides the user through the major workflow steps. Many nf-core
     workflows use the "tube map" design for that. See https://nf-co.re/docs/community/brand/workflow-schematics#examples for examples.   -->
Default workflow steps:

1. Read QC ([FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/))
2. Read trimming ([Trim Galore](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/))
3. Reference index generation and alignment ([Bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml))
4. Annotation conversion ([gffread](https://github.com/gpertea/gffread))
5. Gene-level quantification ([featureCounts / Subread](http://subread.sourceforge.net/))
6. Aggregated reporting ([MultiQC](http://multiqc.info/))

## Installed Modules

Installed nf-core modules (from [modules/nf-core](modules/nf-core)):

1. bowtie2/align
2. bowtie2/build
3. fastqc
4. gffread
5. multiqc
6. subread/featurecounts
7. trimgalore

Installed nf-core subworkflows (from [subworkflows/nf-core](subworkflows/nf-core)):

1. utils_nextflow_pipeline
2. utils_nfcore_pipeline
3. utils_nfschema_plugin

Source of record for installed components and pinned commits: [modules.json](modules.json).

## Parameters

Core run parameters:

1. `--input` (required): Sample sheet CSV describing input FASTQ files.
2. `--fasta` (required): Reference genome FASTA used to build Bowtie2 indexes.
3. `--gff` (required): Annotation file in GFF/GFF3 format used for downstream counting.
4. `--outdir` (required): Output directory for all pipeline results.

Reporting and run metadata:

1. `--multiqc_title`: Custom title for the MultiQC report.
2. `--multiqc_config`: Custom MultiQC config YAML.
3. `--multiqc_logo`: Logo path for MultiQC branding.
4. `--multiqc_methods_description`: Custom methods description YAML for MultiQC.
5. `--email` / `--email_on_fail`: Completion email notifications.

Execution and helper flags:

1. `--help`: Show the standard help output.
2. `--help_full`: Show detailed help output.
3. `--show_hidden`: Include hidden parameters in help output.
4. `--version`: Print pipeline version and exit.

You can provide parameters either on the command line or with a params file.

Example params file (`params.json`):

```json
{
   "input": "samplesheet.csv",
   "fasta": "reference.fasta",
   "gff": "annotation.gff3",
   "outdir": "results",
   "multiqc_title": "RNA-seq Prokaryotes Run"
}
```

## Usage

> [!NOTE]
> If you are new to Nextflow and nf-core, please refer to [this page](https://nf-co.re/docs/get_started/environment_setup/overview) on how to set-up Nextflow. Make sure to [test your setup](https://nf-co.re/docs/get_started/run-your-first-pipeline) with `-profile test` before running the workflow on actual data.


First, prepare a samplesheet with your input data that looks as follows:

`samplesheet.csv`:

```csv
sample,fastq_1,fastq_2,strandedness
CONTROL_REP1,AEG588A1_S1_L002_R1_001.fastq.gz,AEG588A1_S1_L002_R2_001.fastq.gz,forward
CONTROL_REP2,AEG588A1_S2_L002_R1_001.fastq.gz,AEG588A1_S2_L002_R2_001.fastq.gz,reverse
CONTROL_REP3,AEG588A1_S3_L002_R1_001.fastq.gz,,unstranded
```

Each row represents a fastq file (single-end) or a pair of fastq files (paired end).
The `strandedness` column accepts `unstranded`, `forward`, or `reverse`.
If omitted, strandedness defaults to `unstranded`.
If a sample appears on multiple rows, all rows for that sample must use the same strandedness value.

Now, you can run the pipeline using:


```bash
nextflow run trestlebiosciences/rnaseqprokaryotes \
   -profile <docker/singularity/.../institute> \
   --input samplesheet.csv \
   --fasta reference.fasta \
   --gff annotation.gff3 \
   --outdir <OUTDIR>
```

Run with a params file:

```bash
nextflow run trestlebiosciences/rnaseqprokaryotes \
   -profile <docker/singularity/.../institute> \
   -params-file params.json
```

> [!WARNING]
> Please provide pipeline parameters via the CLI or Nextflow `-params-file` option. Custom config files including those provided by the `-c` Nextflow option can be used to provide any configuration _**except for parameters**_; see [docs](https://nf-co.re/docs/running/run-pipelines#using-parameter-files).

## Credits

trestlebiosciences/rnaseqprokaryotes was originally written by @bradfordwinkelman.

We thank the following people for their extensive assistance in the development of this pipeline: @jonwinkelman


## Contributions and Support

If you would like to contribute to this pipeline, please see the [contributing guidelines](docs/CONTRIBUTING.md).

## Citations

<!-- TODO nf-core: Add citation for pipeline after first release. Uncomment lines below and update Zenodo doi and badge at the top of this file. -->
<!-- If you use trestlebiosciences/rnaseqprokaryotes for your analysis, please cite it using the following doi: [10.5281/zenodo.XXXXXX](https://doi.org/10.5281/zenodo.XXXXXX) -->

<!-- TODO nf-core: Add bibliography of tools and data used in your pipeline -->

An extensive list of references for the tools used by the pipeline can be found in the [`CITATIONS.md`](CITATIONS.md) file.

This pipeline uses code and infrastructure developed and maintained by the [nf-core](https://nf-co.re) community, reused here under the [MIT license](https://github.com/nf-core/tools/blob/main/LICENSE).

> **The nf-core framework for community-curated bioinformatics pipelines.**
>
> Philip Ewels, Alexander Peltzer, Sven Fillinger, Harshil Patel, Johannes Alneberg, Andreas Wilm, Maxime Ulysse Garcia, Paolo Di Tommaso & Sven Nahnsen.
>
> _Nat Biotechnol._ 2020 Feb 13. doi: [10.1038/s41587-020-0439-x](https://dx.doi.org/10.1038/s41587-020-0439-x).
