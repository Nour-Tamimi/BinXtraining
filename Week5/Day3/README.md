# Day 3 - Findings & Analysis Report
1. Summary of Steps & Results
StandardScaler: Successfully normalized all numerical features to have a mean of 0 and a standard deviation of 1.

PCA & Variance Selection: Evaluated the cumulative explained variance plot and selected 7 components to retain ~95% of the total dataset variance.

2D Projection & Clustering: Compressed the dataset down to 2 components (n_components=2), applied K-Means with 5 clusters, and plotted the resulting groups.

2. Trade-offs (Preserved vs. Cost)
What Was Preserved:

95% of Information: Retained almost all crucial patterns and variability using just 7 components, boosting model efficiency and eliminating noise.

Visual Interpretability: Reducing to 2 dimensions unlocked the ability to directly visualize and inspect complex multi-dimensional data clusters on a simple 2D plot.

What Was the Cost:

Loss of 5% Variance: Discarded a small fraction of information (mostly noise and minor details).

Loss of Direct Feature Meaning: The principal components (PC1 and PC2) are linear combinations of original features rather than single identifiable variables (like age or blood pressure), making literal interpretation harder.