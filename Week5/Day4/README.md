# Day 4 — t-SNE & Anomaly Detection

## What This Day Covered
Today focused on two things: **visualizing high-dimensional data with t-SNE**, and **finding unusual/rare data points using Isolation Forest**.

---

## 1. t-SNE vs. PCA

Both reduce high-dimensional data to 2D so we can plot it, but they answer different questions:

| | PCA | t-SNE |
|---|---|---|
| Preserves | Global spread/variance | Local neighborhoods (who's near whom) |
| Best for | Compression + modeling | Visualization only |
| Speed | Fast | Slower on large data |
| Axes | Meaningful directions | No real meaning — only relative position |

**Key idea:** PCA shows the overall shape of the data. t-SNE shows which points genuinely belong close together, even if they aren't linearly separable. Use t-SNE to *look*, use PCA (or original features) to *model*.

---

## 2. What Anomaly Detection Is

Anomaly detection finds data points that don't fit the normal pattern — fraud, defects, failures, or (in our lab) abnormal medical readings.

It's usually **unsupervised** because anomalies are rare and rarely labeled ahead of time. Instead of learning "what an anomaly looks like," the model learns "what normal looks like" and flags anything that deviates from it.

---

## 3. Isolation Forest

The core idea: anomalies are easier to isolate than normal points, because they sit apart from the dense mass of data. The algorithm randomly splits the data repeatedly — points that get separated in very few splits are flagged as anomalies.

- **contamination** parameter = your estimate of what fraction of the data is anomalous.
- Output: `-1` = anomaly, `1` = normal.

DBSCAN's "noise" points (from Day 2) work on a similar idea — another simple form of anomaly detection.

---

## 4. Lab Summary (Our Results)

- Applied t-SNE to the dataset and compared it against the Day 3 PCA plot.
- **Finding:** PCA showed clusters overlapping heavily, making them hard to trust. t-SNE confirmed one cluster (yellow) as genuinely distinct, while two others (purple/teal) stayed mixed even after t-SNE — suggesting KMeans likely over-split one real group into two.
- Ran Isolation Forest → **66 anomalies flagged**.
- Inspected two flagged points:
  - One stood out due to a **rare combination** of features (young age + high troponin/blood sugar + low blood pressure).
  - One stood out because of **single extreme values** on their own (CK-MB and troponin both far above normal).
- Cross-checked against the real `Result` label (which the model never saw): **63 of 66 flagged anomalies (95%) were actually positive cases.**

---

## Key Takeaway

Isolation Forest, without ever seeing the true labels, was able to surface medically meaningful outliers almost as well as a supervised model would. This is the core value of anomaly detection: it can catch rare, important events (fraud, disease, failures) without needing labeled training data — but flagged points still need human judgment before acting on them, since the model flags *statistical* rarity, not a guaranteed cause.
