# Day 1 — Supervised Learning Concepts & the Scikit-learn API

## Overview

Today covered the foundational concepts of supervised learning and the standard
workflow used to build and evaluate models with scikit-learn. This forms the base
for every supervised ML task going forward.

## What I Learned

### 1. Supervised Learning
Supervised learning means training a model on labeled data — examples where the
correct answer is already known — so it can learn to predict that answer on new,
unseen data.

### 2. Regression vs. Classification
The two types of supervised learning problems, distinguished by what's being predicted:

### 3. Features (X) and Target (y)
- **X (features)** — the input columns the model learns from
- **y (target)** — the single column of correct answers the model tries to predict


### 4. The Scikit-learn API
Every scikit-learn model follows the same four-step pattern, regardless of the
algorithm underneath:

| Step | Method | Purpose |
|---|---|---|
| 1. Instantiate | `model = Model()` | Create the model, set options |
| 2. Fit | `model.fit(X_train, y_train)` | Learn patterns from training data |
| 3. Predict | `model.predict(X_test)` | Predict on new/unseen data |
| 4. Score | `model.score(X_test, y_test)` | Evaluate performance |

### 5. Train/Test Split
The most important rule in ML: **never evaluate a model on the data it was
trained on.** A model can memorize training data and appear to perform
perfectly, while actually failing on new, real-world data.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```

- `test_size=0.2` holds out 20% of the data for testing
- `random_state=42` fixes the split so results are reproducible
