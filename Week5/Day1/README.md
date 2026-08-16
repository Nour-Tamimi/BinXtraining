# Day 1 — K-Means Clustering

## What I did today
Practiced unsupervised clustering using K-Means on the Iris dataset (numeric version).

## Key concepts learned

**Dataset**
- Iris data has 4 numeric features: sepal length, sepal width, petal length, petal width
- The 5th column (0/1/2) is the species label — it's the answer key, not a feature, so it must be dropped before clustering

**Scaling**
- Features should be standardized (StandardScaler) before clustering so no single feature dominates due to scale differences

**Choosing k (number of clusters)**
- Elbow method: plot inertia vs k, look for the bend
- Silhouette score: measures how well-separated and cohesive clusters are (range -1 to 1, higher is better)
- Compared k=2 and k=3 → k=2 had the higher score (0.57)

**Fitting the model**
- K-Means groups points by minimizing distance to cluster centroids
- Cluster centers are the average feature values of all points in that cluster
- If fit on scaled data, centers need to be inverse-transformed back to real units to be interpretable

**Visualizing clusters**
- Can't plot 4+ features directly, so 2 options:
  1. PCA — compress all features into 2 new axes for plotting
  2. Pick two original features directly (only works cleanly with 2D data)
- Color points by cluster label to see groupings visually

**Checking cluster meaning**
- The plot alone doesn't explain what a cluster "is"
- Compare cluster averages per feature, and cross-tab against true labels (if available) to interpret what each cluster represents

## Next steps
- Try clustering on a dataset with more real-world features (not just Iris)
- Explore other cluster validation metrics
