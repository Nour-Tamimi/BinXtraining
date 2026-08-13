# Day 5 — Scikit-learn Pipelines & Tuned Mini-Project

## What I did

1. **Built a `Pipeline`** chaining a `ColumnTransformer` (preprocessing) and a `DecisionTreeRegressor` (model) into a single object, so every fit/transform happens in the correct order automatically.
2. **`ColumnTransformer` for mixed data**: scaled numeric columns with `StandardScaler` and one-hot encoded categorical columns (`city`, `statezip`, `country`) with `OneHotEncoder` — in one step, instead of handling them manually like in Day 4.
3. **Added real engineered features** (missing from Day 4):
   - `renovated`: binary flag from `yr_renovated` (0 = never renovated, 1 = renovated) — a raw year mixes "never renovated" with actual years in a way a tree can't use well; the flag isolates the signal.
   - `above_ratio`: `sqft_above / sqft_living` — captures the proportion of a home above ground, independent of overall size.
4. **Tuned the whole pipeline with `GridSearchCV`** (5-fold CV), using `model__` prefixes to target the model step's hyperparameters from inside the pipeline.
5. **Evaluated once on the held-out test set** and compared the tuned pipeline against an untuned baseline (`max_depth=5`, same pipeline structure).

## Why this matters (vs. Day 4)

Day 4's biggest lesson was discovering a data leak — I had fit the scaler/encoder on the full dataset before splitting into train/test. Fixing it meant remembering to `fit_transform` on train and `transform` on test, every time, manually.

A `Pipeline` makes that mistake structurally impossible: when `GridSearchCV` cross-validates the pipeline, it refits preprocessing on only each fold's training portion automatically. There's no manual step to forget.

## Key takeaways

- A `Pipeline` isn't just cleaner code — it changes *where* leakage can happen. Preprocessing is now part of what gets cross-validated, not a separate step done once beforehand.
- `ColumnTransformer` lets numeric and categorical columns get different treatment (scaling vs. encoding) in one object that slots directly into the pipeline.
- Tuning a pipeline uses `step_name__param_name` (e.g. `model__max_depth`) so `GridSearchCV` knows which step a hyperparameter belongs to.
- The test set gets touched exactly once — after tuning is finished — to get an honest final number. Everything before that (grid search, comparing hyperparameters) only ever sees training data.
- This structure (EDA → engineered features → `ColumnTransformer` → model → `Pipeline` → `GridSearchCV` → one held-out evaluation) is the same shape the Phase 3 capstone will expect.
