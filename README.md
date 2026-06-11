# DepPrior Reproducibility Repository

This repository supports the manuscript:

**DepPrior: Integrating CRISPR Dependency Predictability with Multi-omics Reproducibility to Prioritize Candidate LUAD Therapeutic Targets.**

It contains the reviewer-facing code and metadata needed to document the computational workflow, data provenance, execution order, software environment, random seeds, and source-data status.

## Minimum Contents Requested for Review

The revision action list specifically requires the public repository to go beyond a placeholder link. This repository therefore includes:

- `README.md`: project overview, data availability, and quick-start notes.
- `RUN_ORDER.md`: recommended notebook and version-capture execution order.
- `DATA_DICTIONARY.md`: dataset provenance and expected input/output files.
- `VERSIONING_AND_SEEDS.md`: software versions, version-capture commands, and random-seed policy.
- `workflow_diagram.mmd`: top-level workflow diagram.
- `requirements.txt` and `environment.yml`: Python/R environment specifications.
- `software_versions_declared.tsv`: declared runtime versions and exact-version capture status.
- `code.ipynb`, `code_r.ipynb`, `code_supplementary.ipynb`, `running_r.ipynb`: cleaned analysis notebooks with outputs removed and no hard-coded local working directory.
- `data/`: manifests for public datasets and expected generated intermediate files.
- `scripts/`: version/session capture utilities.
- `source_data/`: western blot source-data checklist and manifest.

## Repository Layout

```text
.
|-- code.ipynb
|-- code_r.ipynb
|-- code_supplementary.ipynb
|-- running_r.ipynb
|-- DATA_DICTIONARY.md
|-- RUN_ORDER.md
|-- VERSIONING_AND_SEEDS.md
|-- workflow_diagram.mmd
|-- requirements.txt
|-- environment.yml
|-- software_versions_declared.tsv
|-- data/
|   |-- README.md
|   |-- external_data_manifest.csv
|   `-- expected_intermediate_outputs.csv
|-- scripts/
|   |-- collect_versions.py
|   `-- collect_session_info.R
`-- source_data/
    |-- README_western_blot_source_data.md
    `-- western_blot_source_data_manifest.csv
```

## Data Availability

Large public datasets are not redistributed in this repository. Download them from the original sources and place them under the expected local paths before running the notebooks:

- DepMap/CCLE CRISPR dependency, expression, copy-number, and model metadata files under `depmap/`.
- TCGA-LUAD/GDC data processed through `code_r.ipynb` under `data/TCGA/processed/`.
- GEO expression matrices for GSE50081 and GSE72094 under `external/`.
- CPTAC LUAD transcriptomic/proteomic matrices under `external/`.

See `data/external_data_manifest.csv` and `DATA_DICTIONARY.md` for the expected files and their roles.

Uncropped western blot membrane images and densitometry workbooks should be handled as journal source data or supplementary source-data files according to journal and laboratory policy. The public code repository contains only the source-data checklist and manifest.

## Quick Start

Create the analysis environment:

```bash
conda env create -f environment.yml
conda activate depprior
```

If using a separate R installation, use R 4.5.2 and install the R/Bioconductor packages listed in `environment.yml` and `scripts/collect_session_info.R`.

Capture software versions in the final run environment:

```bash
python scripts/collect_versions.py --out logs/python_versions.tsv
Rscript scripts/collect_session_info.R logs/R_sessionInfo.txt
```

Start Jupyter from the repository root and follow `RUN_ORDER.md`.

## Reproducibility Notes

The cleaned notebooks in this repository have execution outputs removed to avoid local path leakage and stale runtime artifacts. Re-run the notebooks from a clean repository root after downloading the required public datasets.

Documented random seeds include NumPy `seed=0`, estimator-level `random_state=0` where applicable, the main train/test split `random_state=42`, and shuffled five-fold cross-validation with a fixed seed.
