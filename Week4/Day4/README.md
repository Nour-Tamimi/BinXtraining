# Day 4 — Feature Engineering & Hyperparameter Tuning

## What I did

1. **Feature engineering**: Inspected and cleaned up the raw features — dropped `date` and `street` after confirming they added no usable signal, and preprocessed the remaining features (scaling numerics, one-hot encoding categoricals) so the model could use them properly.
2. **Hyperparameter grid**: Defined a grid for `DecisionTreeRegressor` covering `max_depth`, `min_samples_split`, `min_samples_leaf`, and `max_features`.
3. **GridSearchCV (5-fold CV)**: Searched the grid and compared against an untuned baseline.
4. **Compared tuned vs. baseline** using cross-validated R², not a single train/test split — this avoids being misled by the variance of one lucky/unlucky split.
5. **Identified the most impactful feature and hyperparameter** using `feature_importances_` and by grouping `cv_results_` by each hyperparameter.

## Feature engineering

1. **Dropped `date`**: split out the year and found every row was the same year, so it carried no signal — removed it.
2. **Dropped `street`**: extremely high-cardinality (near-unique per row), so one-hot encoding it created hundreds of near-empty sparse columns the tree couldn't meaningfully split on given `min_samples_leaf` constraints.
3. **Scaled numeric features / one-hot encoded remaining categoricals** via a `ColumnTransformer` (`StandardScaler` + `OneHotEncoder`) so all features are on comparable scales and categories are usable by the model.

*Note: this pass focused on cleaning and preprocessing existing columns rather than constructing new derived features (e.g. ratios or interactions). If the assignment requires ≥2 newly engineered columns, that step still needs to be added.*

## Results

| Metric | Score |
|---|---|
| CV R² (best_score_) | 0.311 |
| Train R² | 0.255 |
| Test R² | 0.455 |

Compared to my first (less regularized) grid — Train 0.95 / Test 0.51 / CV 0.35 — this version shows **much less overfitting**: train and CV/test scores are much closer together now.

## Investigating: Test R² > Train R²

This looked wrong at first, so I checked for bugs:
- **Preprocessing leak**: originally fit the scaler/encoder on the full dataset before splitting — fixed by fitting only on `X_train` and transforming `X_test` separately. Did not change the pattern.
- **Dropped `street`**: removed the highest-cardinality categorical column in case it was distorting results. Did not change the pattern either.

**Conclusion**: the gap is most likely driven by **R² sensitivity to skewed price outliers** combined with a small test set (~920 rows) — whichever expensive houses happen to land in train vs. test can swing R² noticeably, independent of any code bug. This is documented as an honest finding rather than something resolved.

## Most impactful hyperparameter

Grouping CV scores by each hyperparameter's values (averaged across all other combos), `min_samples_leaf` and `max_depth` produced the largest swings in mean CV R² (~0.084–0.095), while `max_features` barely moved the score. This makes sense — both directly control how much the tree is allowed to fit small, noisy groups of samples.

## Most impactful engineered feature

`sqft_living` (via `feature_importances_`) had by far the highest importance score (~0.69) — the tree relies on it more than any other feature, including the encoded categoricals.

## Key takeaways

- `GridSearchCV`'s `best_score_` is a genuine held-out cross-validated score, not a training score — it's the average across 5 rotating validation folds, not the same as a single fixed test split.
- Always `fit` scalers/encoders on training data only, then `transform` (not `fit_transform`) the test set — fitting on the full dataset before splitting leaks test-set information into preprocessing.
- High-cardinality categorical columns (like `street`) can bloat a one-hot-encoded feature space without adding usable signal, especially once `min_samples_leaf` prevents the tree from splitting on rare categories.
- A train score lower than a test score isn't automatically a bug — with small/skewed datasets, it can reflect real variance in how outliers land across the split.