# Notebooks

The executable workflow associated with the TFM is published as:

- `TFM_Deteccion_Anomalias_Tennessee_Eastman.ipynb`

The repository version preserves the original code-cell order from Google Colab. Stored cell outputs and execution counters were removed to reduce file size, and markdown section headings were added to improve navigation. The computational instructions were not changed.

The notebook covers:

1. Data loading and structural validation.
2. Run-level train/validation partitioning.
3. Standardization using normal training runs only.
4. PCA training and T²/SPE-Q computation.
5. Isolation Forest training and scoring.
6. Dense Autoencoder training and reconstruction-error scoring.
7. Threshold calibration.
8. 3-out-of-5 temporal persistence logic.
9. Independent FaultFree/Faulty Testing evaluation.
10. Precision–Recall/AP analysis.
11. Interpretability.
12. Sensitivity analyses.
13. Final PCA analytical prototype.
14. Tables and figures used in the thesis.

## Execution note

Several cells use Google Drive paths and checkpoints created during development. To reproduce the workflow from scratch, download the public multi-run Tennessee Eastman dataset cited in the root README and adapt `PROJECT_DIR` to the local/Drive location.

A legacy development block comparing this TFM with Yin et al. (2012) is retained only for traceability and is explicitly marked as unsuitable for direct numerical comparison because the evaluation units and protocols differ.
