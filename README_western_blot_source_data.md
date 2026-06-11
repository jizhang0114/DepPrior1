args <- commandArgs(trailingOnly = TRUE)
out <- if (length(args) >= 1) args[[1]] else "logs/R_sessionInfo.txt"
dir.create(dirname(out), recursive = TRUE, showWarnings = FALSE)

sink(out)
cat("R session information\n")
cat("=====================\n\n")
print(sessionInfo())
cat("\nSelected package versions\n")
cat("=========================\n\n")
packages <- c(
  "survival",
  "survminer",
  "TCGAbiolinks",
  "SummarizedExperiment",
  "EnsDb.Hsapiens.v86",
  "ensembldb",
  "GenomicRanges",
  "IlluminaHumanMethylation450kanno.ilmn12.hg19",
  "maftools",
  "ComplexHeatmap",
  "circlize",
  "dplyr",
  "readr",
  "ggplot2",
  "tidyr",
  "tibble",
  "forcats",
  "purrr",
  "patchwork",
  "RColorBrewer"
)
for (pkg in packages) {
  if (requireNamespace(pkg, quietly = TRUE)) {
    cat(pkg, as.character(packageVersion(pkg)), "\n")
  } else {
    cat(pkg, "NOT_INSTALLED\n")
  }
}
sink()
cat("Wrote", out, "\n")
