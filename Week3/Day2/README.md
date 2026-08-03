# ML Learning Notes — Baseline Comparison & Model Evaluation

Summary of what I learned/worked on today.

## Preparing Data for Training

- All values fed into a model must be **numeric** (int/float).
- Non-numeric columns (categories, text, dates) need to be **converted first**, not dropped.

| Data type | How to convert |
|---|---|
| Categorical (few values) | One-hot encoding (`pd.get_dummies`) |
| Categorical (many values, high cardinality) | Frequency encoding, target encoding, or group rare into "Other" |
| Dates | Extract year, month, day, day-of-week, days-since-reference |
| Text | TF-IDF / embeddings |

### Frequency Encoding (used for high-cardinality columns)
Replaces each category with how often it appears in the data (as a proportion).

### Splitting Dates
```python
df["signup_date"] = pd.to_datetime(df["signup_date"])
df["year"]  = df["signup_date"].dt.year
df["month"] = df["signup_date"].dt.month
df["day"]   = df["signup_date"].dt.day
df["day_of_week"] = df["signup_date"].dt.dayofweek
```

##  Feature Strength (Correlation / Coefficients)

The **sign** (+/-) shows direction. The **absolute value** (distance from zero) shows strength.


**Common bug:** `array.sort()` sorts in-place and returns `None`. Use `np.sort(array)` instead, or sort a DataFrame column with `.sort_values()`.

## Evaluating the Model vs Baseline

```python
from sklearn.metrics import mean_squared_error

baseline_value = y_train.mean()
baseline_predictions = np.full_like(y_test, baseline_value)
baseline_rmse = np.sqrt(mean_squared_error(y_test, baseline_predictions))

## Why Compare to a Baseline

A baseline is a "lazy guess" — for regression, it means predicting the **mean of the target** for every single row, no matter what the features say.

**Rule of thumb:** If your model can't beat this lazy guess by a meaningful margin, it hasn't learned anything useful from the data.

**Analogy:** A weather forecaster who always says "70°F" (the average) vs one who actually studies humidity, wind, satellite data, etc. If the second forecaster is barely better than "always 70°F," their analysis isn't adding real value.

```

### My Model's Results
| Metric | Value | Meaning |
|---|---|---|
| MAE | 204,851.06 | Typical prediction error size (in $) |
| RMSE | 990,887.55 | Error size, penalizes big mistakes more — much higher than MAE, meaning some large outlier errors exist |
| R² | 0.037 | Model explains only 3.7% of price variation |
| Baseline RMSE | 1,009,874.42 | Error if I just guessed the average price every time |

### Conclusion
Model RMSE (990,887) vs Baseline RMSE (1,009,874) → only **~1.9% improvement**.

Combined with a very low R² (0.037), **the model is not adding meaningful value** — it performs close to just guessing the average house price for every row.

### Likely Causes to Investigate Next
- Missing or poorly encoded important features (especially location)
- Linear model may not capture non-linear relationships in the data
- Target (price) may be skewed — try predicting `log(price)` instead
- Try a tree-based model (Random Forest, XGBoost) instead of linear regression
