# College Sleep & GPA — Model Tuning Summary

## What I Did

I worked with a dataset on college students' sleep habits and GPA, aiming
to predict `term_gpa` from the other features (sleep patterns, prior GPA,
term load, demographics, etc.).

**Preprocessing:**
- Filled missing values (mode for categorical, mean for numeric)
- Dropped a column that wasn't useful and any remaining nulls
- Split the data into train / validation / test sets
- Scaled numeric features and one-hot encoded categorical ones

**Modeling:**
I trained and tuned five different regression models — Random Forest, KNN,
SVR, Decision Tree, and Linear Regression — checking each one's R² and
RMSE on the validation set only, and adjusting hyperparameters until each
model reached its best validation performance.

## Results

Random Forest came out on top with `n_estimators=9`, giving the highest R²
(0.48) and the lowest RMSE (0.759) on validation. I then evaluated that
final model on the test set exactly once to get an honest final score.

## What I Learned

- **Tuning has to happen on the validation set, not the test set.** If I
  used the test set to pick hyperparameters, the model would essentially
  be memorizing the test data instead of being evaluated on truly unseen
  data — the results would look better than they really are, and I
  wouldn't be able to trust the final score.
- **The test set is for one final check.** It only gets touched once,
  after everything is already decided, so it gives a fair picture of how
  the model would actually perform on new, real data.
- **Comparing multiple models matters.** Not every model fits the data
  equally well — trying several side by side (and tuning each one fairly)
  made it clear which one actually generalized best, rather than just
  picking the first thing that worked.
- **Preprocessing affects everything downstream.** Handling missing values,
  scaling, and encoding properly before training was necessary for models
  like KNN and SVR to work correctly, since they're sensitive to feature
  scale.
