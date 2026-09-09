# Day 4 — Model Integration & Error Analysis

**Model:** MobileNetV2 (transfer learning, frozen base + small trainable head)
**Data:** 900 images (300 per class: plastic, paper, garbage), 720 train / 180 test

## What I did today

### 1. Finished training the model (carried over from Day 3)
- Base MobileNetV2 frozen (2.26M params locked), only a small custom head
  trained on top (82K trainable params) — the core idea of transfer learning.
- Trained with **early stopping** (`monitor="val_loss"`, `patience=3`,
  `restore_best_weights=True`) instead of a fixed epoch count.
- Training ran to epoch 19, then auto-stopped and rolled back to its best
  point: **epoch 16** — val_accuracy 98.3%, val_loss 0.0458.
- Training accuracy hit 100% early on, but validation accuracy stayed high
  and stable rather than dropping — a sign this was *not* meaningful
  overfitting, just the natural point of diminishing returns.

### 2. Integration — built one end-to-end `predict()` function
Wrapped preprocessing + model into a single function: raw image path in,
class name + confidence out.
```python
def predict(image_path):
    processed = preprocess_for_model(image_path)   # same steps as training
    probabilities = model.predict(processed)
    predicted_index = np.argmax(probabilities)
    return class_names[predicted_index]
```
Tested on a real image — correctly predicted "plastic" at 99.6% confidence.

### 3. Training/serving skew — verified prevention
Confirmed the exact same 4 steps (resize 224×224 → BGR to RGB → float32 →
MobileNetV2's `preprocess_input` scaling) are used both when building the
training data (`X`) and inside `predict()`. Any mismatch here would silently
corrupt predictions without throwing an error — this is the #1 thing to
double-check before deploying any model.

### 4. Error analysis — confusion matrix
Ran the trained model on all 180 test images and built a confusion matrix:

| True \ Predicted | garbage | paper | plastic |
|---|---|---|---|
| **garbage** | 60 | 0 | 0 |
| **paper** | 1 | 58 | 1 |
| **plastic** | 0 | 1 | 59 |

Only 3 errors total (98.3% accuracy). No single dominant confusion pair —
garbage was classified perfectly; the few mistakes were spread thin between
paper and plastic in both directions.

### 5. Inspected the 3 misclassified examples individually
| # | True | Predicted | Verdict |
|---|---|---|---|
| 1 | paper | plastic | Model weakness — glossy, rounded-shape paper bag visually resembles plastic |
| 2 | paper | garbage | Data issue — unusual fiery-orange background likely distracted the model |
| 3 | plastic | paper | Model weakness (data-influenced) — beige/matte plastic bag resembles paper's color/texture |

**Pattern spotted:** 2 of 3 errors shared the same unusual fiery background,
suggesting some sensitivity to background noise rather than pure bag-only
confusion — worth checking background variety in training data going forward.

## Key takeaway
A single accuracy number (98.3%) would have hidden all of this. The
confusion matrix showed *where* the model struggles, and manually inspecting
the actual misclassified images revealed *why* — which is the difference
between a superficial evaluation and a real one.

## Next steps to consider
- Train on the full 5,000-per-class dataset instead of the 300-per-class subset.
- Check background variety across the dataset; consider more augmentation for lighting/background variation.
- Unfreeze a few of MobileNetV2's later layers for fine-tuning if more accuracy is needed.
