# Day 3 - Diagnosing and Fixing Model Fit 

## Project Overview
This project demonstrates the bias-variance tradeoff hands-on using the `load_breast_cancer` dataset from scikit-learn. It deliberately overfits, deliberately underfits, and then fixes overfitting with regularization — using both a Decision Tree and a Ridge-regularized linear model.


## Key Concepts Learned

**Overfitting (high variance)**
- Symptom: large train-val gap, train accuracy near 1.0
- Cause: model too complex, memorizes noise instead of the real pattern
- Fix: regularization (limit tree depth, min_samples constraints, Ridge/L2 penalty)

**Underfitting (high bias)**
- Symptom: both train AND validation accuracy are low — gap alone is not the tell
- Cause: model too simple to capture the real pattern
- Fix: increase model complexity, add relevant features, remove excessive regularization

**Ridge (L2) vs Lasso (L1)**
- Ridge shrinks coefficients toward zero but never exactly to zero — reduces model flexibility without dropping features
- Lasso can shrink coefficients to exactly zero — acts as automatic feature selection
- Both combat overfitting; Ridge is used here since we want to retain all features, just shrink their influence

## Common Mistakes Fixed Today
- `model.score()` takes `(X, y)`, not `(y_true, y_pred)` — use `accuracy_score(y_true, y_pred)` for that comparison instead
- `Ridge` (regression) outputs continuous values and can't be scored with classification metrics directly — use `RidgeClassifier` for classification tasks
- A small train-val gap does NOT automatically mean a good fit — check absolute accuracy too, since underfitting also produces small gaps

## Next Steps / Possible Extensions
- Try `LogisticRegression(penalty='l2', C=...)` and compare `C` values against `RidgeClassifier` `alpha` values
- Plot train vs validation accuracy across a range of `max_depth` values to visualize the bias-variance curve
- Try `Lasso`/`L1` regularization and compare which coefficients get zeroed out
