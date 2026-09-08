# Day 3 — Image Preprocessing

**Dataset used:** [Plastic/Paper/Garbage Bag Synthetic Images](https://www.kaggle.com/datasets/vencerlanz09/plastic-paper-garbage-bag-synthetic-images) (Kaggle)
5,000 images per class — Plastic Bag, Garbage Bag, Paper Bag.

## What I learned

**1. Raw images need standardizing before a model can use them**
Different sizes, color formats, and pixel ranges break or degrade training. Preprocessing makes every image consistent.

**2. OpenCV reads images in BGR, not RGB**
`cv2.imread()` loads color channels in Blue-Green-Red order. Forgetting to convert to RGB silently corrupts colors — I saw this happen live on my own dataset image before fixing it with `cv2.cvtColor(img, cv2.COLOR_BGR2RGB)`.

**3. The core preprocessing pipeline (Step 1)**
```python
img = cv2.imread(path)                        # read (BGR)
img = cv2.resize(img, (128, 128))              # standardize size
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)     # fix color order
img = img / 255.0                              # normalize 0-255 -> 0-1
```
Wrapped this into a reusable `preprocess_image()` function and tested it on multiple real images from the dataset.

**4. Data augmentation expands a small dataset (Step 2)**
Used Keras's `ImageDataGenerator` to randomly rotate, zoom, flip, and brighten a single image, generating multiple varied versions on the fly. This is the main defense against overfitting — it forces the model to generalize instead of memorizing exact images.

**5. Preprocessing must match the pre-trained model exactly (Step 3)**
For transfer learning (MobileNetV2), the input must match what the base model was originally trained on:
- Resized to **224×224** (not 128×128)
- Scaled to **-1 to 1** using `preprocess_input()` (not the 0-1 scaling from Step 1)

Verified this on a real image — confirmed pixel range came out as `-1.0 to 0.99`, not `0-1`, proving the correct scaling was applied.

**Key lesson:** Step 1's pipeline (128×128, 0-1) and Step 3's pipeline (224×224, -1 to 1) are *different on purpose*. Using the wrong one at the wrong time is exactly the kind of mismatch that quietly ruins transfer-learning results — this connects directly to tomorrow's topic, training/serving skew.

## Kaggle setup notes (for next time)
- Kaggle now uses an API token (not a downloaded `kaggle.json` file) — generate one under Kaggle Settings → API, then set it with `os.environ["KAGGLE_API_TOKEN"] = "..."` in Colab.
- Always regenerate/expire a token if it's ever been visible in a screenshot or shared anywhere.
- This dataset's zip file had a nested folder structure: `data/Bag Classes/Bag Classes/<class name>/`.

## Next up (Day 4)
Model integration, training/serving skew prevention, and error analysis with a confusion matrix.
