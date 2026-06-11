# Data Dictionary

This file documents dataset provenance, expected local files, analysis roles, and generated intermediate outputs. Exact public dataset versions should be recorded in `logs/` during the final manuscript run.

## DepMap / CCLE

Purpose: gene dependency prediction and candidate target prioritization.

Expected local input directory: `depmap/`

Expected input files:

- `depmap/Model.csv`: cell-line metadata and lineage annotations.
- `depmap/CRISPRGeneEffect.csv`: CRISPR gene-effect matrix.
- `depmap/OmicsExpressionTPMLogp1HumanProteinCodingGenes.csv`: expression features.
- `depmap/OmicsCNGeneWGS.csv`: copy-number features.

Key fields:

- `ModelID` or first metadata column used as the cell-line identifier.
- `lineage`, `primary_disease`, or related tumor-type annotations.
- gene columns containing dependency or molecular feature values.

Processing notes:

- Matrices are aligned by cell-line identifier.
- Feature scaling, filtering, and PCA should be fitted within the training data or training fold only.
- Dependency classification uses gene-effect thresholds documented in the notebooks.
- Model benchmarking reports AUROC, R2, and RMSE.

Expected generated files include:

- `depmap_processed/DepMap_LUAD_Model_subset.csv`
- `depmap_processed/DepMap_LUAD_CRISPRGeneEffect_aligned.pkl`
- `depmap_processed/DepMap_LUAD_Expression_TPMLogp1_aligned.pkl`
- `depmap_processed/DepMap_LUAD_CNGeneWGS_aligned.pkl`
- `depmap_processed/depmap_luad_dataset.npz`
- `results/perf_dep_genelevel_by_model_fig2.csv`
- `results/depmap_pan_scan.csv`

## TCGA-LUAD / GDC

Purpose: tumor-level multi-omics coherence, clinical association, mutation context, and survival analyses.

Expected local output directory from `code_r.ipynb`: `data/TCGA/processed/`

Expected generated files include:

- `TCGA_LUAD_STAR_log2TPM_samples_x_genes.rds`
- `TCGA_LUAD_Masked_Somatic_Mutation.rds`
- `TCGA_LUAD_CNV_geneTSS_matrix.rds`
- `TCGA_LUAD_Methylation_genePromoter_matrix.rds`
- `TCGA_LUAD_Clinical_sample_level.rds`
- `TCGA_LUAD_multiomics_features.rds`
- CSV exports used by Python notebooks, including expression and clinical tables.

Key fields:

- TCGA barcode or `sample_id`.
- `OS.time`: overall-survival time.
- `OS.event`: event indicator.
- mutation status columns for canonical LUAD drivers.
- candidate target gene expression columns.

Processing notes:

- Primary tumor samples are prioritized.
- Samples are aligned by TCGA barcode.
- Survival analyses are exploratory and use Kaplan-Meier/log-rank visualization and Cox proportional hazards models.

## GEO External Transcriptomic Cohorts

Purpose: independent transcriptomic reproducibility assessment.

Expected local input directory: `external/`

Expected input files:

- `external/GSE50081_expr_log2.csv`
- `external/GSE72094_expr_log2.csv`

Processing notes:

- Cohorts are used for LUAD sample-level expression reproducibility checks.
- Rank-based concordance is emphasized because platforms and normalization differ.

## CPTAC LUAD Resources

Purpose: cross-omics reproducibility assessment at RNA/protein levels.

Expected local input directory: `external/`

Expected input files:

- `external/CPTAC_LUAD_expr_log2_bcm.csv`
- `external/CPTAC_LUAD_expr_log2_broad.csv`
- `external/CPTAC_LUAD_expr_log2_washu.csv`
- `external/CPTAC_LUAD_proteomics_log2_bcm.csv`
- `external/CPTAC_LUAD_proteomics_log2_umich.csv`

Processing notes:

- Analyses focus on candidate-gene rank concordance and RNA/protein consistency.
- These analyses assess expression/protein reproducibility, not functional dependency conservation.

## Western Blot Source Data

Purpose: source-data support for selected perturbation experiments.

Expected files for journal source-data submission:

- uncropped membrane/exposure image files for each antibody and biological replicate.
- densitometric calculation workbook with raw band intensities, loading-control normalization, relative expression values, and statistical-test inputs.
- source-data manifest linking each membrane/calculation to the figure panel.

Public repository policy:

- The repository includes the checklist and manifest under `source_data/`.
- Raw membrane images and calculation workbooks should be uploaded to the journal submission system or another approved source-data location if sharing is permitted.
