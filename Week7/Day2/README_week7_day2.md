# Week 7, Day 2 Summary — Building CNNs & Transfer Learning

## What I Learned

**1. Pooling**
After convolution, a pooling layer shrinks the feature map by keeping only the strongest signal in each small region. Max pooling takes the maximum value in each 2×2 window. This reduces computation, helps control overfitting, and makes the network more robust to small shifts in the image. Convolution and pooling alternate to progressively distill the image into compact, meaningful features.

**2. Full CNN architecture**
A typical CNN stacks convolution + pooling blocks to extract features, then flattens the result into a single row of numbers and feeds it into Dense layers (from Week 6) for the final classification. Convolution/pooling layers find the patterns; Dense layers make the final decision from those patterns.

**3. Data Augmentation**
Image datasets are often too small to train a strong CNN without overfitting. Data augmentation artificially expands the dataset by applying random transformations — flips, rotations, zooms — so the network sees more varied versions of the same images and learns the real pattern instead of memorizing exact examples. This is the standard first defense against overfitting in computer vision.

**4. Transfer Learning**
Training a CNN from scratch needs huge amounts of data and compute. Transfer learning reuses a model already trained on millions of images (e.g. MobileNetV2), keeping its learned feature-detecting layers frozen, and replacing only the final classification layer with a new one trained on this project's smaller dataset. This is the most practical technique for getting strong results from limited data, and is the expected approach for this project's image classification task.

## What I Did Today

- Built and trained a small CNN from scratch (Conv2D + MaxPooling2D + Flatten + Dense) on the Melanoma Skin Cancer dataset (Benign vs Malignant) and recorded its test accuracy.
- Added data augmentation (random flip, rotation, zoom) to the same architecture and compared the new training/validation curves against the scratch model.
- Applied transfer learning using a frozen, pre-trained MobileNetV2 as the feature extractor, with only a new final layer trained on this dataset, and compared its accuracy and training time to the previous two models.
- Built a summary comparison table (test accuracy, test loss, epochs to converge) across all three approaches.
- Documented in Markdown which approach performed best and why, tying the result back to the underlying theory (less overfitting risk and faster convergence expected from augmentation and transfer learning respectively).

## Key Takeaway

For an image dataset of limited size (like this project's), transfer learning is expected to outperform a CNN trained from scratch — it starts from feature detectors already learned on millions of images, so it only needs to learn how those features map to benign vs malignant, rather than learning to detect edges and textures from zero.
