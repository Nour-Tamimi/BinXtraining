# Week 5 — Unsupervised Learning & Dimensionality Reduction

## Overview

Week 5 shifted from supervised learning (Weeks 3–4) to **unsupervised learning** — working with
data that has no target/label column. Instead of predicting a known answer, the goal is to discover
hidden structure in the data on its own: natural groupings, underlying dimensions, or unusual points.

| | Supervised | Unsupervised |
|---|---|---|
| Data | Has labels (X and y) | No labels (X only) |
| Goal | Predict the known target | Discover hidden structure |
| Examples | Regression, classification | Clustering, dimensionality reduction, anomaly detection |
| Evaluation | Compare prediction to true label | No ground truth — internal metrics and judgment |

## Topics Covered

### Day 1 — K-Means Clustering
- K-Means partitions data into `k` clusters by repeating: assign points to the nearest centroid,
  then move each centroid to the mean of its assigned points, until stable.
- Choosing `k` is not automatic — two methods were used to pick it:
  - **Elbow method**: plot inertia (within-cluster distance) against `k` and look for the point
    where improvement sharply slows down.
  - **Silhouette score**: a score from -1 to +1 measuring how well each point fits its own cluster
    versus the nearest other cluster. Higher = better-defined clusters. Used to confirm the elbow
    with an actual number rather than just a visual guess.
- Features must be **scaled before clustering** (`StandardScaler`), since K-Means relies on distance
  and an unscaled large-range feature would dominate the result.

### Day 2 — DBSCAN & Hierarchical Clustering
- **DBSCAN** groups points based on density and automatically flags sparse points as noise/outliers
  (label `-1`) instead of forcing every point into a cluster. It doesn't require choosing `k` in
  advance — controlled instead by `eps` (neighbor distance) and `min_samples`.
- **Hierarchical clustering** builds a full tree (dendrogram) of nested clusters, from every point
  as its own cluster up to one single cluster. The tree can be "cut" at any height to get any number
  of clusters after the fact.
- Neither method needs `k` chosen in advance the way K-Means does — a real advantage when the right
  number of groups isn't known.

### Day 3 — PCA (Dimensionality Reduction)
- High-dimensional data causes real problems: sparsity, meaningless distances, easier overfitting,
  and no way to visualize beyond 2–3 dimensions (the "curse of dimensionality").
- **PCA** builds new axes ("principal components") that are combinations of the original features,
  ordered so the first component captures the most variance (information), the second captures the
  next most, and so on.
- **Explained variance ratio** tells you how much information each component keeps. The
  **cumulative sum** of these ratios tells you how much total information is kept after keeping the
  first *n* components — commonly used to decide how many components to keep (e.g. enough to reach
  ~95% cumulative variance).
- PCA requires scaled data, since it's variance-based and an unscaled feature would look artificially
  important.

### Day 4 — t-SNE & Anomaly Detection
- **t-SNE** is a visualization-only dimensionality-reduction technique. Unlike PCA (which preserves
  global variance), t-SNE preserves local neighborhoods — points close together in high dimensions
  stay close in the 2D plot. Its axes have no real meaning, and it should never be used as input to
  a downstream model, only for looking at the data.
- **Anomaly detection** finds points that differ significantly from the norm — often unsupervised
  since anomalies are rare and rarely labeled in advance.
- **Isolation Forest** detects anomalies by measuring how few random splits it takes to isolate a
  point — anomalies isolate quickly since they sit apart from the dense mass of normal data.

## Applied Work — Clustering the Cardiac Dataset

Using the same cleaned heart-attack dataset (1,316 patients, 8 features), three clustering methods
were run and compared, purely on the features (`Result` excluded, since clustering is unsupervised).

**Setup:** Features scaled with `StandardScaler` before clustering, then reduced to 2D with PCA
purely for visualization (not used as clustering input).

### Results

| Method | Clusters Found | Noise Points | Silhouette Score |
|---|---|---|---|
| K-Means (k=5) | 5 | 0 | 0.238 |
| DBSCAN | 2 | 75 | 0.211 |
| Hierarchical (cut at 2) | 2 | 0 | 0.832 |

### Interpretation

- **K-Means** gave the most reasonable result for this dataset. Its silhouette score (0.238) is
  honest and not inflated by an uneven cluster split.
- **DBSCAN** found 2 clusters and flagged 75 points as noise/outliers — useful for spotting unusual
  cases, but its score was close to K-Means, not clearly better.
- **Hierarchical's high score (0.832) is misleading.** Cutting the tree at exactly 2 clusters
  (`t=2`) was a forced choice, not something the algorithm decided on its own — it produced one very
  large cluster and one much smaller one. An uneven split like this inflates the silhouette score
  artificially, since a small, distant cluster is trivially "well separated." A fair comparison would
  test several cut heights and check cluster sizes, not just take the single highest score.
- **Overall:** this dataset does not form strong, well-separated natural clusters — all fair scores
  sit around 0.2–0.25. K-Means is the most reliable and interpretable choice here, but the clinical
  features don't group into clean geometric clusters on their own, even though the data does have a
  real, meaningful clinical outcome label (`Result`).

## Key Takeaways

- Unsupervised methods find structure without labels — useful for exploring a dataset even when a
  target variable already exists elsewhere (like `Result` in this project).
- A single metric (like silhouette score) can be misleading on its own — always check cluster sizes
  and balance before trusting a high score.
- PCA and t-SNE solve different problems: PCA compresses and preserves global structure (usable for
  modeling); t-SNE is for visualization only.
- Scaling before clustering or PCA is not optional — both are distance/variance-based and unscaled
  features will dominate the result.
