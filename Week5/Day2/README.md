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
Built a dendrogram and cut it into 5 clusters. Silhouette score came out
close to others (0.214).

## Conclusion
None of the three methods found strong, clearly separated clusters — all
honest scores landed around 0.2–0.25. This suggests the medical features
in this dataset don't form clean geometric groups, even though the data
has a real clinical outcome label. K-Means gave the most reliable and
interpretable result of the three.

## Next Steps
- Tune DBSCAN's eps using a k-distance plot instead of guessing
- Try RobustScaler instead of StandardScaler, since medical data often
  has extreme outliers that distort distance-based clustering
