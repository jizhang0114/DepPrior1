# Versioning and Random Seeds

## Declared Analysis Runtime

The revised manuscript reports:

- Python 3.12.6
- R 4.5.2
- Bioconductor 3.22 where applicable

Major Python packages:

- pandas
- NumPy
- SciPy
- scikit-learn
- XGBoost
- lifelines
- matplotlib
- seaborn
- statsmodels
- scanpy
- gseapy
- omicverse

Major R packages:

- survival
- survminer
- TCGAbiolinks
- SummarizedExperiment
- maftools
- ComplexHeatmap
- dplyr/readr/tidyr/tibble
- ggplot2

Before journal resubmission, generate exact version records from the final execution environment:

```bash
python scripts/collect_versions.py --out logs/python_versions.tsv
Rscript scripts/collect_session_info.R logs/R_sessionInfo.txt
```

## Random Seeds

Documented manuscript seeds:

- NumPy initialization: `np.random.seed(0)` where stochastic NumPy operations are used.
- PCA and tree-based estimators: `random_state=0` where applicable.
- Main held-out train/test split: `random_state=42`.
- Five-fold cross-validation: shuffled K-fold splitting with a fixed seed.

## Determinism Checklist

- Fit preprocessing only on training folds.
- Apply fitted preprocessing objects to validation/test folds.
- Use identical train/test partitions when comparing models for a gene.
- Record package versions and operating system details at final execution.
- Export all random seeds and split indices where feasible.
- Keep run logs with date, command, input data version, output file names, and notebook/script hash when possible.
- Re-run notebooks from the repository root because the public notebooks do not set any machine-specific working directory.
