# Breast Cancer Classification — Model Comparison

## Overview
This project applies and compares four classification algorithms on the **Breast Cancer Wisconsin dataset** (`sklearn.datasets.load_breast_cancer`) to predict whether a tumor is **malignant** or **benign**.

The models compared are:
- K-Nearest Neighbors (KNN)
- Decision Tree
- Random Forest
- Support Vector Machine (SVM)

---

## Dataset
- **Source:** `sklearn.datasets.load_breast_cancer`
- **Samples:** 569
- **Features:** 30 numeric features (e.g., radius, texture, perimeter, area, smoothness)
- **Target:** Binary — `0 = malignant`, `1 = benign`

---

## Models

### 1. K-Nearest Neighbors (KNN)
A non-parametric, instance-based algorithm. It classifies a new sample based on the majority class among its *k* closest neighbors in feature space.
- **Pros:** Simple, no training phase, works well with clean, well-scaled data.
- **Cons:** Slow at prediction time on large datasets, sensitive to feature scaling and irrelevant features.

### 2. Decision Tree
A tree-based model that splits data recursively based on feature thresholds that best separate the classes (e.g., using Gini impurity or entropy).
- **Pros:** Easy to interpret and visualize, handles non-linear relationships, no need for feature scaling.
- **Cons:** Prone to overfitting, high variance — small changes in data can produce a very different tree.

### 3. Random Forest
An ensemble of many decision trees, each trained on a random subset of data and features (bagging). The final prediction is made by **majority vote** across all trees.
- **Pros:** Reduces the variance and overfitting of a single decision tree, generally more accurate and robust.
- **Cons:** Less interpretable than a single tree, more computationally expensive, slower to predict.

### 4. Support Vector Machine (SVM)
Finds the optimal hyperplane that maximizes the margin between classes. Can use kernels (e.g., RBF) to handle non-linear decision boundaries.
- **Pros:** Effective in high-dimensional spaces, works well when there's a clear margin of separation.
- **Cons:** Sensitive to feature scaling, less efficient on large datasets, harder to tune (kernel/parameters).

---

## Results

| Model            | F1 Score | Notes |
|-------------------|----------|-------|
| KNN               |**0.965** |
| Decision Tree     |**0.957** |
| **Random Forest** | **0.97** | **Best performer** |
| SVM               | **0.959**     | 

---
## Conclusion
Among the four models tested, **Random Forest** provided the best balance of precision and recall for this dataset, making it the most reliable choice for this classification task. Simpler models like KNN and Decision Trees are faster and more interpretable but tend to be less robust, while SVM can perform well but requires careful tuning and feature scaling.
