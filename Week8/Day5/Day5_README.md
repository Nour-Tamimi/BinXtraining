# Day 5 — Full Evaluation, Explainability & Sprint Review

**Model:** MobileNetV2 (transfer learning), 3-class bag classifier
**Data:** 900 images (300 per class), 720 train / 180 test — retrained fresh today, best epoch 7 (val_loss 0.1655)

## 1. Full evaluation vs. baseline

| Class | Precision | Recall | F1-score |
|---|---|---|---|
| garbage | 0.95 | 0.97 | 0.96 |
| paper | 0.98 | 0.92 | 0.95 |
| plastic | 0.89 | 0.93 | 0.91 |

**Overall accuracy:** 93.9% (180 test images)

**Baseline comparison:** a "dumb" baseline that always guesses the most
common class ("plastic") scores **33.3%** accuracy (classes are perfectly
balanced, 60/60/60, so this is exactly 1/3). The real model's 93.9% is a
clear, unambiguous improvement over blind guessing.

**Weakest metric:** plastic precision (0.89) — the model over-predicts
"plastic" more than it should, more often than it does for the other two
classes.

## 2. Handling imbalance — not applicable

This step (SMOTE, precision-recall trade-off) is designed for imbalanced
tasks like fraud/churn detection. This dataset's test set is perfectly
balanced (60 images per class), so there is no minority class to correct
for. Skipped intentionally, and documented here rather than forced in.

## 3. Explainability with SHAP

Applied `shap.GradientExplainer` to generate per-pixel importance maps for
individual predictions.

**Example 1 — correct prediction** (true: plastic, predicted: plastic):
SHAP importance was concentrated on the bag itself, not the (fiery-orange)
background — evidence the model is genuinely learning bag features, not
just background context, at least in this case.

**Example 2 — misclassified** (true: paper, predicted: plastic):
SHAP showed the shiny metallic background did influence the "garbage"
class's importance map, while the paper-vs-plastic confusion itself traced
back to the bag's ambiguous shape and translucent color — a case of
**model weakness driven by bag appearance**, not purely a background
distraction.

**Takeaway:** errors don't share one single root cause. Some relate to
background noise (seen Day 4), others to genuine visual ambiguity in the
bag's shape/color/texture. This nuance would be invisible from the
accuracy number or confusion matrix alone — SHAP made it visible.

## 4. Sprint Review

- Full pipeline demoed: preprocessing → integrated `predict()` → evaluation
  metrics vs. baseline → confusion matrix → per-prediction SHAP explanations.
- Notebook runs cleanly end-to-end from a fresh Colab session (data
  download → training → evaluation → explainability).
- Imbalance-handling step explicitly addressed as not applicable, with
  reasoning documented rather than skipped silently.

## 5. Retrospective

**What went well:** the full pipeline — preprocessing, training with early
stopping, integration into one `predict()` function, evaluation against a
baseline, and SHAP explainability — works end-to-end and is reproducible
from a brand-new notebook.

**One concrete change for Sprint 4 (deployment):** train on the full
5,000 images per class instead of the current 300-per-class subset, since
the model's weakest metric (plastic precision, 0.89) is most directly
addressed by more training data, before moving into deployment and final
polish.
