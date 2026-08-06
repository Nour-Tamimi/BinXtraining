#  Day 5 -Term GPA Prediction — Project Notes

## Overview
This project predicts `term_gpa` (a continuous target) from student features such as `sleep_midpoint_clock` and `first_generation`, using a regression workflow with proper train/test hygiene to avoid data leakage.

---

## 1. Data Preprocessing

### Missing Values

### Encoding Categorical Variables
| Method | When used |
|---|---|
| One-Hot Encoding | Categories with no natural order |
| Label Encoding | Ordinal categories (small → medium → large) |
| Frequency Encoding | High-cardinality columns |

Encoders are always **fit on `X_train` only**, then applied with `.transform()` on `X_test` (`handle_unknown='ignore'` to avoid errors on unseen categories).

### Feature Scaling
- `StandardScaler` fit on `X_train`, applied to both `X_train` and `X_test`.
- Target (`term_gpa`) also scaled where needed, using a **separate** scaler instance, with `.reshape(-1, 1)` since sklearn requires 2D input.

### Golden Rule
`fit_transform()` on training data only. `transform()` only on test data — never re-fit on test, to prevent data leakage.

---
## 2. Modeling & Evaluation

### Baseline
Since `term_gpa` is continuous, this is a **regression** task — the baseline is a `DummyRegressor` that always predicts the mean of `y_train`.
Baseline RMSE: **0.9625**

### Models Trained
- Decision Tree Regressor
- Linear Regression

### Metrics Used
Regression tasks use **RMSE** and **R²** (not F1/precision/recall — those are classification-only metrics).

---

## 3. Day 5 — Mini-Project Results

### Round 1: Initial Results (Before Leakage Fix)
| Model | RMSE | R² |
|---|---|---|
| Baseline (mean) | 0.9625 | — |
| Linear Regression | 0.00064 | 0.99999954 |

An R² this close to 1.0 was a red flag for **data leakage**. Investigation found two problem columns:
- `gpa_change` — almost certainly derived directly from `term_gpa` (e.g. `term_gpa - prior_gpa`), giving the model a near-exact arithmetic shortcut instead of a real pattern to learn.
- `student_id` — an arbitrary identifier mistakenly included and scaled as a numeric feature, adding noise rather than signal.

Both columns were dropped and the models were retrained.

### Round 2: Results After Removing Leakage

**Linear Regression**
The R² of 0.42 means the model explains about 42% of the variance in `term_gpa` — a moderate result. It captures a real relationship in the data, but there's still a lot of variation in GPA it can't account for from these features alone, so predictions won't be reliable for every unseen case. The RMSE of 0.72 is noticeably lower than the baseline RMSE of 0.96, meaning the model's predictions are meaningfully closer to the true values than simply guessing the average — it has learned something useful, just not enough to be highly precise.

**Decision Tree**
The R² of -0.26 is negative, meaning this model performs worse than simply predicting the average `term_gpa` for every student — it has not learned a useful pattern and is overfitting to the training data. The RMSE of 1.06 is even higher than the baseline RMSE of 0.96, confirming the model is underperforming compared to a naive guess.

**Decision Tree (max_depth=5)**
After limiting depth to 4, R² improved from -0.26 to 0.09 and RMSE dropped to 0.91 (just below the baseline of 0.96). This confirms the earlier negative R² was overfitting. Still weaker than Linear Regression (RMSE 0.72).

**Conclusion:** Linear Regression remains the better model for predicting `term_gpa` on this dataset.

---

## Next Steps
- [x] Audit feature list for leakage sources
- [x] Re-run Decision Tree and Linear Regression after fixing leakage
- [x] Compare final RMSE/R² of both models against the baseline
- [x] Try limiting Decision Tree depth to reduce overfitting
- [x] Document final model choice and reasoning
