# Day 3 -Logistic Regression & Classification Metrics 

Summary of concepts covered today, using a weather prediction example (rainy = 0, sunny = 1) and the scikit-learn breast cancer dataset (malignant = 0, benign = 1).

---

## 1. How Logistic Regression Computes Probability

```
temperature (x)
      ↓
z = β₀ + β₁·x        ← linear combination (dot product), also called log-odds
      ↓
σ(z) = 1 / (1 + e^(-z))   ← sigmoid function, squashes z into a probability [0, 1]
      ↓
P(sunny)
      ↓
compare with threshold
      ↓
final decision: 0 or 1
```

- **z** can range from −∞ to +∞, so it can't be used directly as a probability.
- The **sigmoid function** converts z into a value between 0 and 1.
- The result is an S-shaped curve: low at small x, high at large x, transitioning around the point where z = 0.

---

## 2. Threshold

The probability alone isn't a decision — the **threshold** is the cutoff point that turns a probability into a yes/no classification.

- Default threshold: **0.5**
  - P ≥ 0.5 → classify as 1
  - P < 0.5 → classify as 0
- The threshold can be moved depending on which type of error is more costly:
  - Lower threshold → catches more positives, but more false alarms
  - Higher threshold → fewer false alarms, but misses more real positives

---

## 3. Odds & Log-Odds (Logit)

- **Odds** = P / (1 − P) → compares "yes" directly to "no" instead of to the whole population.
  - Odds = 1 → 50/50
  - Odds > 1 → positive class more likely
  - Odds < 1 → negative class more likely
- **Log-Odds (logit)** = log(Odds) → spreads odds evenly across −∞ to +∞, which is exactly what the linear part of the model (β₀ + β₁·x) predicts.
- The sigmoid function is the inverse of the logit — it converts log-odds back into a normal probability.

---

## 4. Likelihood & Maximum Likelihood Estimation (MLE)

- **Likelihood function**: a number measuring how probable the actual observed data is, given a specific set of coefficients (β₀, β₁).
  - Computed by multiplying the individual probabilities the model assigned to each true outcome.
  - In practice, the **log-likelihood** is used instead (sum of logs) for numerical stability and easier calculus.
- **MLE**: the process of searching for the β₀, β₁ values that maximize the likelihood — i.e., the coefficients that make the real observed outcomes as probable as possible under the model.

Training phase (once) → MLE finds best β₀, β₁
Prediction phase (every new input) → uses those fixed β₀, β₁ to compute z → σ(z) → probability

---

## 5. Why Accuracy Alone Is Misleading

On imbalanced data (e.g., 95% no-churn, 5% churn), a model that always predicts the majority class scores 95% accuracy while being completely useless — it never identifies the minority class it's actually meant to detect.

This is why richer metrics (precision, recall, F1, AUC-ROC) are needed.

---

## 6. Precision & Recall

- **Precision**: Of everything predicted positive, how many were actually positive? (Are the alarms trustworthy?)
- **Recall**: Of everything actually positive, how many did the model catch? (Did it miss anything?)

Improving one often comes at the cost of the other.

---

## 7. F1 Score

```
F1 = 2 × (Precision × Recall) / (Precision + Recall)
```

- The **harmonic mean** of precision and recall — heavily penalizes imbalance between the two.
- A model can't fake a good F1 by excelling at only one metric while failing the other.
- F1 close to 1 → both precision and recall are good.
- F1 close to 0 → at least one of them is bad.

---

## 8. ROC Curve & AUC

- **ROC Curve**: plots True Positive Rate (Y-axis) vs. False Positive Rate (X-axis) at *every possible threshold*.
  - Each point on the curve corresponds to one specific threshold.
- **AUC (Area Under the Curve)**: summarizes the entire ROC curve as a single number.
  - AUC = 0.5 → random guessing (no better than a coin flip)
  - AUC = 1.0 → perfect classifier
  - Intuition: AUC = probability that a randomly chosen positive case gets a higher predicted score than a randomly chosen negative case.
- **Key property**: AUC is independent of any single threshold — it evaluates ranking quality across all thresholds combined, making it ideal for comparing classifiers.

### AUC-ROC Result
- AUC ≈ 0.99 → near-perfect ability to rank malignant cases above benign cases across all thresholds.
- This confirms the model's probability estimates are reliable in general — but threshold-specific metrics (especially recall) still need to be checked before real-world deployment.

---
