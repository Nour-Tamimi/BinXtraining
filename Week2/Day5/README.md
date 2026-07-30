# Data Analysis Notes — July 30, 2026

A summary of today's topics covered: statistics concepts, chart types, and Python/pandas string & date handling.

---

## 1. Correlation Coefficient

Measures how two variables move together, from **-1** to **+1**.

*
**Reading the result:**
| Value | Meaning |
|---|---|
| +1 | Perfect positive relationship |
| -1 | Perfect negative relationship |
| 0 | No linear relationship |
| ~0.7 | Fairly strong positive relationship |
| ~0.2 | Weak relationship |

⚠️ Correlation only captures **linear** relationships — a strong curved relationship can still show near-zero correlation.

---

### Line chart (price over time)

### Scatter plot (relationship between two variables)

sns.scatterplot(data=med, x='bmi', y='charges', hue='smoker')

## 2. Describing a Histogram

- **Shape**: symmetric, skewed right/left, uniform, bimodal/multimodal
- **Center**: mean vs. median (median more robust to outliers)
- **Spread**: range, standard deviation
- **Outliers**: unusual values far from the rest
- **Modality**: number of peaks

**Example description:**
> "The histogram is right-skewed, with most values clustered between 20–40 and a long tail extending toward 100. The median (28) is lower than the mean (35), confirming the skew."

---

## 3. Describing a Scatter Plot

- **Direction**: positive, negative, or no clear trend
- **Form**: linear, curved, clustered, no pattern
- **Strength**: strong / moderate / weak (ties back to correlation)
- **Outliers**: points far from the main cluster
- **Groups**: differences when colored by `hue`

**Example description:**
> "The scatter plot shows a weak-to-moderate positive relationship between BMI and charges. When colored by smoking status, smokers show a steeper positive trend and higher charges at every BMI level."

---

