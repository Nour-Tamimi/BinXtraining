# Day 2 — Cross-Validation

## What I Learned Today

### 1. Why a single train/test split isn't enough
One split is just one random division of the data — it can be a lucky or
unlucky roll of the dice. A model can look great or bad just because of
which rows happened to land in the test set that one time.

### 2. How k-fold cross-validation works
- The training data is split into k equal folds (e.g. k=5).
- The model trains k times, each time using a different fold as the
  validation set and the rest for training.
- Every data point gets used for validation exactly once and for
  training k-1 times — no data is wasted.
- Averaging the k scores gives a more stable, trustworthy estimate than
  any single split.

### 3. Reading `cross_val_score` results
```python
scores = cross_val_score(model, X_train, y_train, cv=5, scoring='r2')
```
- `scores` → one score per fold
- `scores.mean()` → the overall performance estimate
- `scores.std()` → how consistent the model is across folds

**Rule of thumb:** high mean + low std = a model you can trust.
High mean + high std = maybe just lucky on some folds.

### 4. Applied this to my Random Forest regression model
- Fold scores: `[0.32, 0.44, 0.41, 0.004, 0.30]`
- Mean R²: **0.296**, Std: **0.154**
- The std is large relative to the mean, and one fold (0.004) is a clear
  outlier → the model's performance is inconsistent, not reliable.
- Compared to Day 1's single-split R² (0.48): the single split
  overestimated how well the model generalizes. The CV mean is the more
  trustworthy number — the real model is weaker and less stable than
  the Day 1 result suggested.

### 5. Stratified k-fold — for classification only
- On classification tasks, plain k-fold can accidentally create folds
  with very different class proportions (e.g. one fold ends up with
  almost none of a certain class).
- Stratified k-fold fixes this by keeping the same class ratio in every
  fold.
- Sklearn does this automatically when `cv` is an integer and the model
  is a classifier.
- Checked this on the breast cancer dataset: 212 malignant (37.3%) vs
  357 benign (62.7%) — a mild imbalance. Confirmed with `StratifiedKFold`
  that each fold kept roughly this same 37/63 ratio.

### 6. Why stratification doesn't apply to regression
Stratified k-fold needs discrete classes to preserve ratios of. Regression
targets are continuous (e.g. GPA from 0–4) — there's no fixed set of
"classes" to balance across folds, so there's no ratio to preserve.
Plain k-fold is used instead. (If you really wanted balanced value ranges
across folds, you'd have to manually bin the continuous target first —
not something done by default.)

s