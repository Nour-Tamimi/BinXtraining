# Descriptive Statistics — What I Learned Today

## Why this comes before modeling
A model is basically a compressed summary of patterns in data. Before trusting a model's output, I need to know the data's own center, spread, and shape — otherwise I can't tell if a prediction looks "normal" or way off.

## Central tendency (the "typical value")

| Measure | What it is | Best for |
|---|---|---|
| Mean | Arithmetic average | Symmetric data, no big outliers |
| Median | Middle value when sorted | Skewed data / data with outliers (robust) |
| Mode | Most frequent value | Categorical data, or finding the most common value |

- Mean gets **pulled** toward outliers. Median doesn't.
- If mean and median are close, that's a sign there's likely no major skew/outliers.
- If they diverge a lot, that gap is the outlier's fingerprint.

```python
import numpy as np
data = np.array([10, 12, 12, 13, 100])
np.mean(data)    # 29.4 — pulled up by the outlier
np.median(data)  # 12.0 — unaffected
```

**Mode isn't in numpy** — use one of these instead:
```python
from scipy import stats
stats.mode(data, keepdims=False)      # scipy

import pandas as pd
pd.Series(data).mode()                 # pandas — handles ties (multiple modes)

from collections import Counter
Counter(data).most_common(1)          # pure python
```

## Spread (how scattered the values are)

| Measure | Meaning | Robust to outliers? |
|---|---|---|
| Range | Max − Min | No |
| Variance | Average **squared** distance from the mean | No |
| Std dev | √variance — same units as the data | No |
| IQR | Q3 − Q1, width of the middle 50% | Yes |

**How variance/std dev are actually computed (step by step):**
1. Find the mean
2. Find each value's deviation (value − mean)
3. Square each deviation (removes negative signs, punishes big gaps harder than small ones)
4. Average the squares → **variance**
5. Take the square root → **standard deviation** (back in original units, so it's the one people usually report)

```python
np.var(data)   # variance
np.std(data)   # standard deviation
```

**IQR** — sort the data, then IQR = Q3 − Q1 (the width of just the middle 50%). It ignores the top and bottom quarter entirely, which is exactly why an outlier can't distort it.

```python
q1, q3 = np.percentile(data, [25, 75])
iqr = q3 - q1
```

**Rule of thumb for why they differ:** variance/std dev use *every* point's exact distance from the mean (so one outlier can dominate the whole calculation). IQR only uses two positions in the sorted list (so an outlier just sits harmlessly outside the window being measured). Mean pairs naturally with std dev; median pairs naturally with IQR — both members of each pair use the same kind of information (magnitude vs. position).

## Percentiles and quartiles
A percentile is the value below which a given % of data falls. Q1 (25th), median (50th), and Q3 (75th) split sorted data into quarters — exactly what a box plot draws (box = middle 50%, whiskers = min/max, and whiskers are the part most sensitive to outliers).

## Is the spread actually "big"? → Coefficient of Variation (CV)
Std dev alone is meaningless without context — 14 could be huge or tiny depending on the mean. Compare std to the mean:

```
CV = (std / mean) × 100%
```

| CV | Interpretation |
|---|---|
| < ~15% | Low spread — tightly clustered |
| ~15–35% | Moderate spread |
| > ~35% | High spread — widely scattered |

Example: age column, mean = 39, std = 14 → CV ≈ 36% → high spread relative to its own center.

Quick sanity check without any formula: **mean ± 1 std** gives a rough "typical range" — e.g. 39 − 14 = 25 to 39 + 14 = 53.


## Key takeaways
- Center answers "what's typical here?" — spread answers "how much does it vary from that?" Always report both.
- Mean/std dev are precise but outlier-sensitive. Median/IQR are coarser but robust.
- A std dev number means nothing on its own — compare it to the mean (CV) to know if the data is actually spread out or tightly clustered.
- Always check `value_counts()` and clean inconsistent categories before computing any of the above.
