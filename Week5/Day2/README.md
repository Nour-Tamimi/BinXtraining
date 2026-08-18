# Clustering Lab — Comparing K-Means, DBSCAN, and Hierarchical Clustering

## What I Did
I took a medical dataset and clustered it three different ways to see
which method best captures the natural structure in the data.

## Methods Used

**K-Means**
Picked k=5 after comparing several values with the elbow method and
silhouette score. Result: silhouette score of 0.238.

**DBSCAN**
Grouped points by density instead of picking a k. Found 2 clusters and
flagged 75 points as noise/outliers. Silhouette score: 0.211.

**Hierarchical Clustering**
Built a dendrogram and cut it into 2 clusters. Silhouette score came out
very high (0.832), but this is likely misleading — a 2-cluster cut often
produces one large group and one tiny group, which inflates the score
artificially. Needs to be re-checked at a matching cluster count (5) for
a fair comparison.

## Conclusion
None of the three methods found strong, clearly separated clusters — all
honest scores landed around 0.2–0.25. This suggests the medical features
in this dataset don't form clean geometric groups, even though the data
has a real clinical outcome label. K-Means gave the most reliable and
interpretable result of the three.

## Next Steps
- Re-run Hierarchical at 5 clusters to compare fairly against K-Means
- Tune DBSCAN's eps using a k-distance plot instead of guessing
- Try RobustScaler instead of StandardScaler, since medical data often
  has extreme outliers that distort distance-based clustering
