# Data Dictionary — Heart Attack Prediction Dataset

**File:** `heart_disease_uci.csv`
**Unit of observation:** One row = one patient record

---

| # | Column Name | Type | Description | Unit / Range | Notes |
|---|---|---|---|---|---|
| 1 | `Age` | Numeric (int) | Patient's age | Years | 14 - 103 |
| 2 | `Gender` | Categorical (binary) | Biological sex of the patient | 0 = Female, 1 = Male | Not a numeric quantity — use bar chart/value_counts, not histogram |
| 3 | `Heart Rate` | Numeric (int) | Number of heartbeats per minute | bpm | Normal resting range ~60–100; very low/high values may be real (bradycardia/tachycardia) or errors — verify against clinical plausibility |
| 4 | `Systolic Blood Pressure` | Numeric (int) | Pressure in arteries when the heart contracts (top BP number) | mmHg | Normal ~90–120; hypertension typically >130–140 |
| 5 | `Diastolic Blood Pressure` | Numeric (int) | Pressure in arteries between heartbeats (bottom BP number) | mmHg | Normal ~60–80; should always be lower than systolic for the same row — good consistency check |
| 6 | `Blood Sugar` | Numeric (float/int) | Patient's blood glucose level | mg/dL | Fasting normal ~70–100; >125 suggests diabetes 
| 7 | `CK-MB` | Numeric (float) | Cardiac enzyme (creatine kinase-MB) released during heart muscle damage | ng/mL (typical) | Normally near-zero in healthy patients; elevated values indicate cardiac injury — expect strong right skew |
| 8 | `Troponin` | Numeric (float) | Highly specific protein biomarker for heart muscle injury | ng/mL (typical) | Most clinically important biomarker here; near-zero when healthy, spikes sharply with heart attack — expect strong right skew, likely the strongest predictor |
| 9 | `Result` | Categorical  — **Target variable** | Outcome label: whether the patient experienced a heart attack | negative = No heart attack, positive = Heart attack  | This is the prediction target for modeling

---

## Notes for EDA

- **Numeric/continuous columns:** `Age`, `Heart Rate`, `Systolic Blood Pressure`, `Diastolic Blood Pressure`, `Blood Sugar`, `CK-MB`, `Troponin`
- **Categorical columns:** `Gender`, `Result` (target)
- **Expected skew:** `CK-MB` and `Troponin` are very likely right-skewed — this is clinically meaningful, not noise. Consider log-transform for visualization/modeling, but do not blindly remove high values as outliers.
- **Consistency checks to run:**
  - `Diastolic Blood Pressure` < `Systolic Blood Pressure` for every row
  - `Age` within a plausible human range
  - No negative values in any of the biomarker/vital columns
- **Class balance:** check `Result` value counts before modeling — if imbalanced, accuracy alone will be a misleading metric later.

