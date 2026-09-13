# Sprint 4 — Day 1: Planning, Serialization & MLOps

**Goal for Sprint 4:** Deploy the bag classifier as a live application with a public URL.

## 1. Sprint 4 Planning

**Sprint goal:** Turn the trained, evaluated model (Sprint 3) into a usable,
publicly deployed application.

**Backlog:**
1. Serialize the model and preprocessing objects (done today)
2. Build a serving script/API around `predict()`
3. Build a minimal UI (upload an image, get a prediction)
4. Deploy to a public URL
5. Polish

**Carried forward from Sprint 3 retrospective:** the plastic-class precision
(0.89) was the weakest metric — retraining on the full 5,000 images per
class (instead of today's 300/class) is planned as a polish step before
final deployment, to strengthen this before it goes live.

## 2. Model Serialization

Two files saved to disk, so the model can be loaded and used without
retraining:

| File | What it is | Why it's needed |
|---|---|---|
| `bag_classifier_model.keras` | The trained model (architecture + weights) | Lets a fresh app load and run the model |
| `label_encoder.joblib` | Maps class index (0,1,2) back to name (garbage/paper/plastic) | Without it, predictions come back as meaningless numbers |

```python
model.save("bag_classifier_model.keras")
joblib.dump(label_encoder, "label_encoder.joblib")
```

## 3. Reload Verification

Loaded both files back in a way that simulates a completely separate
application (no dependence on notebook variables), then ran the same known
test image from Day 4 through it:

```python
loaded_model = tf.keras.models.load_model("bag_classifier_model.keras")
loaded_label_encoder = joblib.load("label_encoder.joblib")
```

**Result:** predicted "plastic" correctly (confidence 0.77). Confirms the
saved files work correctly on their own — no training/serving skew.

*(Note: confidence was lower than Day 4's 0.996 on the same image because
no random seed was fixed at the start of the notebook. Without a seed, both
the model's initial random weights and the random train/test split change
every time the notebook is rerun — this is exactly the reproducibility gap
today's lesson (1.4) warns about. Fix: set a fixed seed, e.g.
`tf.random.set_seed(42)` and `np.random.seed(42)`, at the top of the
notebook before building or training the model.)*

## 4. Pinned Requirements

Exact library versions used during training, locked down for deployment so
the serving environment matches training exactly:

```
tensorflow==2.20.0
opencv-python==5.0.0
scikit-learn==1.6.1
joblib==1.6.0
numpy==2.1.3
```

## Files ready for submission today
- `bag_classifier_model.keras`
- `label_encoder.joblib`
- `requirements.txt`
- The notebook (.ipynb) containing all code through Day 5

## Next up
Build the serving script/API, then a minimal UI, then deploy to a public URL.
