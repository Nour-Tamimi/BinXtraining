# Week 3 — Supervised Learning

## What This Week Covered
This week focused on moving from data exploration into actual model building — training models that predict outcomes, and learning how to judge whether those predictions are trustworthy.

## Key Areas

**Preprocessing**
Getting raw data ready for a model: handling missing values, encoding categorical columns into numbers, and scaling features — all fit on the training set only, to avoid leaking information into evaluation.

**Regression**
Predicting continuous values (like a GPA or a price). Evaluated using error-based metrics that show how far off predictions are from the real values, and compared against a simple baseline to confirm the model actually learned something.

**Classification**
Predicting categories instead of numbers. Learned why raw accuracy can be misleading, and how to read a confusion matrix to understand different types of mistakes a model makes.

**Model Comparison**
Trained more than one model on the same data split and compared them fairly using the same metric, rather than trusting a single model's result in isolation.

**Baselines**
Every model's performance was checked against a baseline — a simple, "dumb" reference point. A model is only meaningful if it clearly outperforms that baseline.

## Takeaway
The main lesson of the week: a model's score only means something in context — relative to a baseline, evaluated on unseen data, and using a metric that actually fits the type of problem (regression vs. classification).
