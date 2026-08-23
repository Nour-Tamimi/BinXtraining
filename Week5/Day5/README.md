# Phase 3 Capstone — Project Kickoff & Sprint 1 Plan
## Cardiac Patient Monitoring System

## 1. Problem Statement
Predict whether a patient experienced a heart attack (`Result`: positive/negative) using 8 clinical
features — age, gender, vitals (heart rate, blood pressure), blood sugar, and two cardiac biomarkers
(CK-MB, Troponin) — from a 1,319-row dataset. The goal is a reliable, well-evaluated binary
classifier, with particular attention to **recall** (catching real positive cases), since missing a
heart attack case (a false negative) is far more costly than a false alarm.

## 2. Definition of Done

Restated and kept visible for the full project, per the professional baseline:

- [X] A clean, documented Jupyter Notebook (or set of notebooks) covering the full pipeline:
      EDA → preprocessing → modeling → evaluation
- [X] A trained model with reported metrics (accuracy, precision, recall, F1, ROC-AUC)
- [X] A GitHub repo with a clean README, `requirements.txt`, and model artifacts
- [ ] A short technical write-up (findings, limitations, model choice reasoning)


## 3. Sprint 1 Goal

> **Understand the data and establish a baseline model to beat.**

Already largely achieved from earlier work (M1–M5): dataset cleaned, EDA complete, Logistic
Regression baseline established, Random Forest trained as a comparison
model with an honest overfitting caveat documented, and a reusable Pipeline built. Sprint 1 for the
capstone re-frames this existing work under the formal backlog/acceptance-criteria structure below,
and closes any remaining gaps (repo setup, branch workflow) before Sprint 2 begins.

## 4. Sprint 1 Backlog

| # | Task | Effort (est.) | Status |
|---|---|---|---|
| 1 | Dataset selection & data dictionary | ✅ Done |
| 2 | Data cleaning (missing values, duplicates, invalid values) |  ✅ Done |
| 3 | EDA & visualizations (distributions, outliers, correlations) | ✅ Done |
| 4 | Baseline classifier (Logistic Regression) + first metrics | ✅ Done |
| 5 | Comparison classifier (Random Forest) + cross-validation | ✅ Done |
| 6 | Confusion matrix + plain-language FP/FN interpretation | ✅ Done |
| 7 | Feature engineering + Scikit-learn Pipeline | ✅ Done |
| 8 | Set up GitHub repo (README skeleton, feature-branch workflow) | ✅ Done |
| 9 | Log baseline metrics formally in repo (`output/model_comparison.csv`) | included in #8 | ✅ Done|


