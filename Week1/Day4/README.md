# Day 4 — What I Learned: Pandas & Real Data Cleaning

Today I moved from arrays to actual tabular data, and this is the first day that felt like "real" data science work instead of just learning syntax.

## What I did
I loaded a CSV into a DataFrame and spent time just looking at it before doing anything — checking its shape, column names, data types, and summary statistics. Then I practiced selecting specific columns, filtering rows by a condition, and finally cleaning the data: counting missing values, deciding how to handle them, dropping duplicates, and using `groupby` to summarize the data by category.

## What clicked for me
- The habit of *never* jumping straight into analysis is the biggest thing I'm taking from today. `df.head()`, `df.info()`, and `df.describe()` before touching anything else already feels like it's going to save me from a lot of confusion later — I caught a missing-values problem in about 30 seconds just by running `df.info()` first.
- Filtering with `df[df["age"] > 30]` felt like a direct continuation of the boolean masking from NumPy yesterday, just applied to a table instead of an array. Making that connection myself was satisfying — it's the same idea wearing a different outfit.
- `groupby` was the "aha" moment of the day. Splitting data into groups, applying an aggregation to each, and getting the results back in one line replaced what I imagine would've been a painful manual loop. I can already picture using this constantly.
- I also had to actually make a judgment call today — whether to fill missing values with the mean or just drop those rows — and realized that's a real decision with tradeoffs, not just a syntax choice. I don't think I fully understand yet when one is clearly better than the other.

## Code I want to remember
```python
import pandas as pd

df = pd.read_csv("data.csv")
df.info()                                          # always look first
df[df["age"] > 30]                                 # filter rows
df["age"].fillna(df["age"].mean(), inplace=True)    # one way to handle missing data
df.groupby("category")["income"].mean()             # split-apply-combine
```

## Still a bit fuzzy
- When to fill missing values vs. drop them entirely — I want to look into this more, since today I filled without a strong justification beyond "it seemed reasonable."
- `.loc` vs `.iloc` — I used `.loc` today but I'm not 100% confident yet on when I'd reach for `.iloc` instead.

## Proof of work
- [x] Loaded a real CSV and reported shape, columns, dtypes
- [x] Counted missing values per column and handled them with a documented reason
- [x] Filtered to a meaningful subset and described what I found
- [x] Used `groupby` to compute and interpret an aggregate statistic
