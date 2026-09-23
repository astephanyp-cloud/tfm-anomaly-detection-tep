# TFM — Multivariate Anomaly Detection on the Tennessee Eastman Process

Repository associated with the Master's Thesis:

**Sistema de alerta temprana para detección de anomalías multivariables: aplicación al Tennessee Eastman Process**

**Author:** Angie Stephany Pineda García  
**Programme:** Máster Universitario en Análisis de Datos Masivos  
**University:** Universidad Europea de Madrid  
**Academic year:** 2025–2026

## Overview

This project designs and evaluates an interpretable early-warning system for multivariate industrial-process anomaly detection using the Tennessee Eastman Process (TEP) as a reproducible benchmark.

Three unsupervised detectors are compared under a common experimental protocol:

- **PCA** with Hotelling's T² and SPE/Q statistics.
- **Isolation Forest**.
- **Dense Autoencoder** based on reconstruction error.

Complete simulation runs are separated across training, validation and testing to avoid information leakage. The evaluation includes threshold calibration, a **3-out-of-5 temporal persistence rule**, false-alarm analysis, detection delay, Precision–Recall metrics and model-specific interpretability.

## Dataset

The study uses the multi-run Tennessee Eastman Process dataset published by Rieth et al.

- 52 process variables used as model inputs:
  - 41 measured variables (XMEAS).
  - 11 manipulated variables (XMV).
- 500 independent simulation runs per condition.
- Sampling interval: 3 minutes.
- FaultFree Training: 500 × 500 samples.
- Faulty Training: 20 faults × 500 × 500 samples.
- FaultFree Testing: 500 × 960 samples.
- Faulty Testing: 20 faults × 500 × 960 samples.

The original dataset is not redistributed in this repository.

Dataset reference:

> Rieth, C. A., Amsel, B. D., Tran, R., & Cook, M. B. (2017). *Additional Tennessee Eastman Process Simulation Data for Anomaly Detection Evaluation*. Harvard Dataverse. https://doi.org/10.7910/DVN/6C3JR1

## Experimental design

The detectors are trained only with normal-operation data. The main configuration decisions were fixed during development before the independent testing stage.

### PCA

- 36 principal components.
- Explained variance: 95.84%.
- T² limit: 58.5475.
- SPE/Q limit: 6.2145.
- Point alarm: T² or SPE/Q above its corresponding limit.
- Final temporal persistence: 3 anomalous samples within the last 5 samples.

### Isolation Forest

- 300 trees.
- `max_samples=2048`.
- `max_features=1.0`.
- `bootstrap=False`.
- `random_state=42`.
- Final score threshold: 0.464063.
- Temporal persistence: 3-out-of-5.

### Autoencoder

- Architecture: 52–32–16–8–16–32–52.
- ReLU hidden activations and linear output.
- Adam optimizer, learning rate 0.001.
- MSE reconstruction loss.
- Batch size: 512.
- Best epoch in the controlled final reproduction: 25.
- Final reconstruction-error threshold: 0.833585.
- Temporal persistence: 3-out-of-5.

## Final independent-testing results

| Model | Global detection (%) | Faults ≥99% | Weighted delay (min) | Persistent FAR (%) | Episodes / 100 h | Mean AP |
|---|---:|---:|---:|---:|---:|---:|
| PCA | 92.951 | 17 | 139.743 | 0.254 | 1.783 | 0.874 |
| Isolation Forest | 94.515 | 13 | 313.393 | 1.676 | 4.650 | 0.785 |
| Autoencoder | 93.529 | 17 | 164.372 | 0.269 | 1.954 | 0.851 |

PCA was selected during development as the main detector because it provided the best operational balance among sensitivity, detection delay, persistent false alarms and interpretability. Faults **3, 9 and 15** remained the least separable scenarios.

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── README.md
├── notebooks/
│   └── README.md
├── results/
│   ├── evaluacion_final_Faulty_Testing_checkpoint.csv
│   ├── PrecisionRecall_20_fallas_Testing.csv
│   ├── metricas_PR_Faulty_Testing_checkpoint.csv
│   ├── resumen_final_completo_modelos_testing.csv
│   ├── seleccion_final_modelo_TFM.csv
│   └── Resumen_PrecisionRecall_modelos.csv
├── models/
│   └── README.md
└── figures/
    └── README.md
```

## Reproducibility

The final CSV artifacts used to construct the thesis result tables are included in `results/`.

The original Google Colab notebook will be added to `notebooks/` after export from the execution environment. This is intentional: the repository should contain the exact notebook used to produce the reported experiments, not a reconstructed approximation.

The raw TEP data are intentionally excluded because they are publicly available from the cited source and are substantially larger than the source code and result artifacts.

## Software

The project was developed in Python/Google Colab using NumPy, pandas, scikit-learn, TensorFlow/Keras, SHAP, Matplotlib, PyReadR and PyArrow.

The saved scikit-learn objects were generated with **scikit-learn 1.6.1**, and the Autoencoder artifact was saved with **Keras 3.13.2**.

## Scope

This repository documents an academic proof of concept. The reported performance corresponds to the Tennessee Eastman Process benchmark and must not be interpreted as validation on a real industrial plant.
