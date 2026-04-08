# TFG-Signify
Automated reporting and predictive analytics system for project-based decision-making using SAP Deepdive reports, Power BI and Machine Learning.
# Automating Reporting and Visual Analytics for Project-Based Decision-Making

**Author:** Carlos Junquera Valdivia  
**Institution:** IE University — Bachelor in Data Business Analytics (DBA)  
**Company:** Signify EMEA  
**Date:** May 2026

---

## Project Overview

This repository holds the technical implementation of the Bachelor in Data Business Analytics – Final Project at IE University. The project designs and implements an end-to-end analytical framework and transforms weekly SAP Deepdive exports into a unified decision support system for project managers at Signify EMEA.

The framework integrates four components:
1. **Automated data ingestion** — SharePoint is linked through Power Query and will aggregate weekly Deepdive images into a long-term panel dataset.
2. **Financial feature engineering** — Six analytical ratios derived from raw SAP variables to capture WBS financial health.

3. **Machine learning risk classifier** — Random Forest model predicting financial risk at the WBS level (ROC-AUC: 0.98, Recall: 0.89).
4. **Power BI dashboard** — Four-page interactive dashboard with automated daily refresh and full slicer synchronisation

---

## Repository Structure

```
├── data/               # Data folder (confidential — see data/README.md)
├── powerbi/            # Power BI dashboard documentation
├── ml_model/           # Python ML pipeline (Google Colab notebook)
├── requirements.txt    # Python dependencies
└── README.md           # This file
```

---

## Key Results

| Metric | Logistic Regression | Random Forest |
|--------|-------------------|---------------|
| ROC-AUC | 0.60 | **0.98** |
| Precision (at-risk) | 0.04 | **0.56** |
| Recall (at-risk) | 0.30 | **0.89** |
| F1-score (at-risk) | 0.08 | **0.69** |

**Risk Distribution across 39,166 WBS observations:**
- High Risk (>0.70): 1,159 (3.0%)
- Medium Risk (0.40–0.70): 86 (0.2%)
- Low Risk (<0.40): 37,921 (96.8%)

---

## Technology Stack

- **Data Source:** SAP PS Module — Deepdive Report (.xlsm)
- **Storage:** Microsoft SharePoint
- **ETL & Dashboard:** Microsoft Power BI (Power Query + DAX)
- **ML Pipeline:** Python (pandas, scikit-learn, imbalanced-learn)
- **Development Environment:** Google Colab

---

## Data Confidentiality

All data used in this project is proprietary to Signify EMEA and cannot be shared publicly. The `data/` folder contains only a README explaining the data structure. The ML model was trained on real operational data under corporate confidentiality agreement.

---

## Contact

Carlos Junquera Valdivia  
IE University — Bachelor in Data Business Analytics
