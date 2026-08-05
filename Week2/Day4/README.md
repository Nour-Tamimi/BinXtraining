#   Day 4 - EDA Lesson Notes — Univariate Analysis, Outliers & Relationships

## 1. What is EDA and why it comes first

**EDA (Exploratory Data Analysis)** means looking closely at your data — its shape, distributions, and problems — *before* building any model.

Think of it like a doctor examining a patient before prescribing treatment. Skipping this step is one of the most common causes of failed ML projects, because a model can only be as good as the data understanding behind it. EDA is where you catch broken, weird, or misleading data before it silently corrupts everything downstream.

---

## 2. Seaborn — statistical visualization

Seaborn sits on top of matplotlib but is built specifically for statistics. Same plotting idea, less code, cleaner defaults, and it understands DataFrames directly.

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.histplot(data=df, x="mine_production_tonnes")
plt.show()
```

---

## 3. Univariate analysis — one variable at a time

"Univariate" = looking at one column by itself.

| Tool | Function | What it shows |
|---|---|---|
| Histogram | `sns.histplot()` | Shape of a **numeric** variable's distribution |
| Box plot | `sns.boxplot()` | Median, quartiles, and outliers at a glance |
| Count plot | `sns.countplot()` | Frequency of each category in a **categorical** variable |
| KDE plot | `sns.kdeplot()` | A smoothed curve version of a histogram |

**Key rule:** histogram/KDE → numeric columns (tonnage, price, age). Countplot → categorical/binary columns (country, disruption flag).

### Making a histogram readable
```python
plt.figure(figsize=(12, 6))
sns.histplot(data=df, x="mine_production_tonnes", bins=50)
plt.title("Mine Production Tonnes", fontsize=16)
plt.xlabel("Tonnes", fontsize=12)
plt.ylabel("Count", fontsize=12)
plt.show()
```

### Spacing bars for discrete/categorical numeric data (e.g. years)
```python
sns.histplot(data=df, x="year", discrete=True, shrink=0.8)
```
- `discrete=True` → one bin per unique value (fixes uneven bin-width bugs)
- `shrink=0.8` → shrinks each bar to 80% width, creating visible gaps

### Handling heavily skewed data (e.g. tonnage, price)
A plain histogram on skewed data (mostly small values, a few huge ones) piles everything into one giant bar. Fix with **log-spaced bins**:
```python
import numpy as np

bins = np.logspace(
    np.log10(df["mine_production_tonnes"].min()),
    np.log10(df["mine_production_tonnes"].max()),
    60
)
plt.hist(df["mine_production_tonnes"], bins=bins)
plt.xscale("log")
plt.show()
```
Multiple bumps in the resulting histogram (multimodal distribution) are normal — it usually means your data naturally clusters into a few size categories (e.g. small vs mid vs industrial-scale mines).

---

## 4. Box plots — simply

Line up your data from smallest to biggest (like kids by height):

- **The box** = the middle 50% of values (the "normal" middle bunch)
- **The line inside the box** = the median (the exact middle value)
- **The whiskers** = the normal-range shortest/tallest values still part of the group
- **The dots past the whiskers** = **outliers** — values so extreme they don't fit the normal pattern

```python
sns.boxplot(data=df, x="mine_production_tonnes")
```

---

## 5. Outlier detection — the IQR method

**Step 1 — Find Q1 and Q3**
```python
Q1 = df["mine_production_tonnes"].quantile(0.25)
Q3 = df["mine_production_tonnes"].quantile(0.75)
```

**Step 2 — Calculate IQR (width of the normal middle 50%)**
```python
IQR = Q3 - Q1
```

**Step 3 — Calculate the fences**
```python
lower_fence = Q1 - 1.5 * IQR
upper_fence = Q3 + 1.5 * IQR
```

**Step 4 — Filter for outliers**
```python
outliers = df[(df["mine_production_tonnes"] < lower_fence) |
              (df["mine_production_tonnes"] > upper_fence)]
```

**Step 5 — Inspect**
```python
print(f"Q1: {Q1}, Q3: {Q3}, IQR: {IQR}")
print(f"Lower fence: {lower_fence}, Upper fence: {upper_fence}")
print(f"Number of outliers: {len(outliers)}")
outliers
```

The dots on a box plot are exactly the rows this formula flags. Being flagged doesn't automatically mean "delete it" — it could be a genuine rare event (keep) or a data-entry error (fix/remove). Note: on heavily skewed data, IQR can flag a *lot* of rows, since it assumes a roughly symmetric distribution.

---

## 6. Class imbalance

When one category in your target variable massively outnumbers another.

**Simple example:** 100 animal photos — 95 dogs, 5 cats. A model can just guess "dog" every time and be 95% "accurate" while never correctly identifying a single cat. It learned to ignore the rare thing you actually wanted it to find.

**Check with:**
```python
sns.countplot(data=df, x="disruption_next_year")
```
If one bar dwarfs the other, that's imbalance. Common in rare-event columns like `disruption`, `high_supply_risk`.

**Common fixes (vocabulary only):** oversampling the minority class, undersampling the majority class, or using precision/recall instead of plain accuracy.

---

## 7. Correlation

Tells you: when one variable goes up, what does the other tend to do?

- **Positive correlation** → both move together (e.g. demand growth ↑ → price ↑)
- **Negative correlation** → they move opposite ways (e.g. years of reserves ↑ → supply risk ↓)
- **No correlation** → no pattern; one tells you nothing about the other

```python
df["years_of_reserves"].corr(df["supply_risk_score"])
```
Returns a number from **-1 to 1**: near +1 = strong positive, near -1 = strong negative, near 0 = no relationship.

---

## 8. Reading a correlation heatmap

```python
sns.heatmap(df.corr(numeric_only=True), annot=True, cmap="coolwarm")
```

- Rows and columns list the same variables — each square compares one pair.
- **Diagonal is always 1.0** (a variable vs itself) — ignore it.
- **Dark red** = strong positive, **dark blue** = strong negative, **pale/white** = weak or none.
- The grid is mirrored across the diagonal — you only need to read half of it.
- Scan for dark squares (excluding the diagonal) — those are your interesting variable pairs worth digging into.

---

## 9. Pair plots

A grid of **every scatter plot you could make**, all at once.

```python
sns.pairplot(df[["years_of_reserves", "price_usd_per_tonne", "supply_risk_score"]])
plt.show()
```

- **Off-diagonal squares** → scatter plots between two different variables
- **Diagonal squares** → histogram of that variable against itself (since a straight line would be meaningless)

A heatmap gives you the correlation *number*. A pairplot lets you *see the actual shape* of the relationship — useful because some relationships are real but not a straight line, which a correlation number alone can miss.
