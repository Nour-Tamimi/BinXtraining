# Week 4 — Evaluation, Tuning & Pipelines

## What this week was really about

Going from "a model that runs" to "a model I can actually trust." Every day built on the last: split honestly → validate reliably → diagnose fit → engineer and tune → wrap it all in a leak-free pipeline.

## Day-by-day, in my own words

**Day 1 — Train/val/test split**
Learned why checking the test set repeatedly while tuning quietly ruins it as an honest measure — you end up fitting your decisions to that one set without realizing it. The fix is a third set (validation) used for all tuning, with the test set opened exactly once at the end.

**Day 2 — Cross-validation**
One validation split can be lucky or unlucky, especially on smaller data. k-fold rotates which portion is held out so every row gets validated once, and averaging across folds gives a far more stable estimate than any single split — plus the standard deviation tells you how much to trust that average.

**Day 3 — Bias-variance**
The two failure modes are opposites: underfitting looks bad on train *and* validation (model too simple), overfitting looks great on train but bad on validation (model memorized noise). The train-vs-validation gap is the diagnostic — I saw this firsthand this week when my own model showed a big gap and had to work out whether it was genuine overfitting or just split variance on a small dataset.

**Day 4 — Feature engineering & GridSearchCV**
Better features often move the needle more than a fancier model. Also spent real time here relearning that hyperparameters (set before training) aren't the same as parameters (learned during training), and that GridSearchCV's `best_score_` is a genuine cross-validated score, not a training score — a distinction I initially found confusing.

**Day 5 — Pipelines**
The lesson that tied everything together. Manually scaling/encoding before splitting is exactly how leakage sneaks in — I'd made this mistake earlier in the week without realizing it. A `Pipeline` + `ColumnTransformer` makes leakage structurally impossible, because preprocessing gets refit on only the training portion of each fold automatically, every time.

## Biggest takeaway

Most of this week's real lessons came from mistakes, not the lessons themselves: a preprocessing leak I didn't catch until digging into a confusing train/test score gap, and realizing high-cardinality categorical columns can quietly break a model's ability to use them well. Pipelines are the professional answer to both — they remove the manual steps where these mistakes happen in the first place.
