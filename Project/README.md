# Cardiac Patient Monitoring System — AI & ML Project

Individual AI/ML project: exploratory data analysis and supervised classification on a heart attack
prediction dataset, built for the BinX Tech AI & Machine Learning Track.

## 1. Project Objective

Predict whether a patient experienced a heart attack (`Result`: positive/negative) from 8 clinical
features — vitals and cardiac biomarkers — using a clean, reproducible EDA + modeling workflow.

## 2. Dataset

- **Source:** [Heart Attack Dataset (Tarik A. Rashid) — Kaggle](https://www.kaggle.com/datasets/tarikarashid/heart-attack-dataset), file `Medicaldataset.csv`
- **Rows:** 1,319 (1,316 after removing 3 rows with a physiologically impossible `Heart rate` value)
- **Target:** `Result` — `positive` (heart attack) / `negative` (no heart attack), moderately
  imbalanced at ~61% positive / 39% negative
- **Features:** Age, Gender, Heart rate, Systolic blood pressure, Diastolic blood pressure,
  Blood sugar, CK-MB, Troponin
- **Data dictionary:** see `data/data_dictionary.md`

## 3. Project Structure

```
Cardiac Patients/
├── data/
│   ├── data_dictionary.md          # column descriptions, expected ranges
│   ├── medicaldataset.csv          # raw dataset
│   └── heart_disease_clean.csv     # cleaned dataset (output of notebook 1)
├── notebooks/
│   ├── 01_data_preparation.ipynb   # loading, cleaning, EDA, statistics
│   └── 02_modeling.ipynb           # supervised modeling, evaluation, pipeline
├── output/
│   ├── eda_01.png ... eda_12.png   # saved EDA charts
│   ├── model_01.png, model_02.png  # saved confusion matrices
│   └── model_comparison.csv        # metrics table, all models side by side
├── requirements_ml.txt
└── README.md
```

## 4. How to Run

1. Create and activate a virtual environment (Python 3.10+ recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate      # Windows: venv\Scripts\activate
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements_ml.txt
   ```
3. Launch Jupyter and run the notebooks **in order**:
   ```bash
   jupyter notebook
   ```
   - `notebooks/01_data_preparation.ipynb` — run top to bottom first. It reads
     `data/medicaldataset.csv` and writes `data/heart_disease_clean.csv`.
   - `notebooks/02_modeling.ipynb` — run top to bottom after notebook 1. It reads
     `data/heart_disease_clean.csv`.

Both notebooks run start to finish with no manual steps, using `random_state=42` everywhere a random
split or model is involved, so results are identical on every rerun.

## 5. Data Preparation & EDA — Summary (Notebook 1)

- No missing values, no duplicate rows.
- `Gender` and `Result` are the only categorical columns; `Gender` is already numerically encoded
  (0 = female, 1 = male), so no additional encoding was needed there.
- **Data quality issue found and fixed:** 3 rows had `Heart rate = 1111` bpm — physiologically
  impossible for a living person — removed as data entry errors.
- **Distribution shapes:**
  - Age, Systolic BP, Diastolic BP → close to normal, mild outliers judged to be within an
    acceptable clinical range and kept.
  - Blood sugar, CK-MB, Troponin → strongly right-skewed.
- **CK-MB and Troponin outliers were deliberately kept, not removed.** Grouping both columns by
  `Result` shows clearly higher values in the positive group — the "outliers" are real clinical
  signal (elevated cardiac biomarkers), not data errors. A log-transform view was used to check the
  shape without altering the underlying values used for modeling.
- Correlation/pairplot analysis (`sns.pairplot` colored by `Result`) was used to look at how features
  relate to each other and to the target.

## 6. Modeling — Summary (Notebook 2)

- **Problem type:** binary classification.
- **Split:** single 80% train / 20% test split, stratified by `Result`, `random_state=42`.
- **Preprocessing:** features scaled with `StandardScaler`, fit on train only.
- **Models trained:**
  1. **Logistic Regression** (baseline)
  2. **Random Forest** (comparison model, 200 trees)
- **Evaluation:** accuracy, precision, recall, F1, ROC-AUC, confusion matrix, and 5-fold stratified
  cross-validation for both models.
- **Pipeline:** a `Scikit-learn Pipeline` (`StandardScaler` → `LogisticRegression`) was built and
  fit directly on raw, unscaled data, then re-validated with cross-validation run on the pipeline
  object itself — confirming the whole workflow is repeatable with no manual steps in between.

### Results (test set)

| Metric | Logistic Regression | Random Forest |
|---|---|---|
| Accuracy | 0.792 | 0.992 |
| Precision | 0.828 | 0.988 |
| Recall | 0.833 | 1.000 |
| F1 Score | 0.831 | 0.994 |
| ROC-AUC | 0.889 | 0.998 |

Full metrics, including 5-fold cross-validation results and the pipeline's numbers, are in
`output/model_comparison.csv`.

### Confusion matrix — plain-language interpretation

- **False positive** (predicted heart attack, actually negative) → unnecessary follow-up tests or
  patient stress — costly and inconvenient, not dangerous on its own.
- **False negative** (predicted no heart attack, actually positive) → a real heart attack case is
  missed — the more serious error for a medical screening task, since it directly risks patient safety.

Given this, **recall** matters more than raw accuracy when judging which model is preferable here.

## 7. Key Finding & Model Choice — Read Before Trusting the Random Forest Score

Random Forest scores near-perfectly on the test set (99.2% accuracy, 100% recall) and **exactly**
100% on the training set — checked explicitly in the notebook as an overfitting test. This gap
(perfect on train, still very high but slightly lower on test) combined with Troponin and CK-MB
almost perfectly separating positive/negative cases on their own (verified via
`df.groupby('Result')[['CK-MB','Troponin']].describe()`) means Random Forest is very likely
memorizing that separation rather than learning a meaningfully more sophisticated pattern.

**This is an honest limitation, not a strength to advertise at face value.** For that reason:
- Logistic Regression's more modest ~79–83% scores are the more trustworthy, generalizable estimate
  of real-world performance.
- The final Pipeline was deliberately built around Logistic Regression rather than Random Forest.
- If reporting Random Forest's numbers, this overfitting risk should be stated alongside them.

## 8. Limitations

- Dataset size is modest (1,316 rows after cleaning); results may not generalize to broader or
  different patient populations.
- Class imbalance (~61/39) means metrics beyond accuracy were prioritized, but a larger, more
  balanced dataset would give more reliable estimates.
- Random Forest's near-perfect scores likely reflect the biomarkers' strong inherent separability
  in this dataset rather than a validated real-world diagnostic tool — see Section 7.
- This is an educational project. It does not constitute clinical diagnosis or medical advice and
  must not be used for real patient decisions (see Out-of-Scope in the project guide).
- No hyperparameter tuning was performed — both models were run with default/simple settings to
  focus on establishing a correct baseline and comparison methodology first.

## 9. Next Steps (Not Yet Implemented)

- Unsupervised analysis: clustering and/or PCA on the feature set (Milestone M6).
- Optional: additional feature engineering, hyperparameter tuning, or saving a trained model
  artifact to `models/`.
