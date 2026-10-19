# IBS microbiome analysis and machine learning

This project analyses public gut microbiome sequencing data for IBS and healthy-control samples. It combines 16S processing, taxonomy and diversity summaries with exploratory classification. The scripts and historical result snapshot are included so the workflow can be reviewed without downloading the dataset first.

## Dataset

- NCBI BioProject: PRJNA637763
- SRA study: SRP266324
- Expected sample set in the metadata extraction: 111 samples (85 IBS, 26 healthy controls)
- Amplicon region: V1–V2; Illumina MiSeq paired-end data

See [DATA_PROVENANCE.md](docs/DATA_PROVENANCE.md) for the source and labeling rules.

## Analysis

QIIME 2 is used for read processing, Deblur denoising, SILVA taxonomy assignment and alpha-diversity summaries. The Python analysis aggregates reads at genus level and compares L1 logistic regression with random forest using stratified five-fold cross-validation.

The preprocessing steps that select features and scale the model input are fitted within each training fold. Full-cohort feature rankings are exploratory and are not used as evidence of predictive performance.

## Run locally

The full workflow downloads many public FASTQ files and the QIIME 2 container image. It can require substantial network transfer, disk space and runtime.

```bash
bash scripts/02_run_ibs_analysis.sh
```

The workflow writes new results to `results_actual/`; the checked-in `results/` directory is a historical snapshot.

## GitHub Actions

Pushes run quick checks on script syntax and confirm that the committed reference outputs and documentation are present. To run the full data-download and QIIME 2 analysis in GitHub Actions, open the Actions tab and manually run **IBS microbiome analysis**. Only a completed full analysis produces the `project02-ibs-results` artifact. A passing fast check is not a claim that the full analysis ran.

## Interpretation

This is an observational microbiome analysis. Associations and cross-validation scores do not establish causality or clinical diagnostic performance. Sample labels are inherited from the source metadata and need to be interpreted within that dataset's design.
