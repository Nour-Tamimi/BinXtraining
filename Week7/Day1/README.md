Week 7, Day 1 Summary — Sprint 2 Kickoff & Convolution
What I Learned

1. Why Dense networks fail on images A 200×200 color image has 120,000 numbers. Feeding that into a Dense layer would require millions of weights just for the first layer — computationally hopeless, and it throws away a key fact about images: nearby pixels are related, and a pattern (an edge, a shape) means the same thing wherever it appears.

2. Convolution — the core idea A convolution slides a small filter (kernel, e.g. 3×3) across the image, computing a dot product at each position — the same dot product from Week 2, applied locally and repeatedly. Each filter learns to detect one specific pattern (an edge, a curve, a texture). The output is a feature map showing where that pattern was found in the image.

3. Why convolution wins over Dense layers

Parameter sharing: the same small filter (e.g. just 9 weights for a 3×3 filter) is reused across the entire image, instead of needing a separate weight for every pixel.
Translation invariance: because the filter slides everywhere, a pattern is detected no matter where it appears in the image.

4. The hierarchy a CNN learns on its own Early layers detect simple patterns (edges). Middle layers combine those into shapes and textures. Deep layers recognize whole objects or complex patterns (e.g. an irregular lesion border). Each layer works on the feature maps of the layer before it, not the raw image — this is how complexity builds up automatically through training, without hand-designing any of it.

5. Padding (valid vs same) Without padding (valid), the filter can't fully cover pixels at the image edges, so the output feature map is slightly smaller than the input. With padding (same), a border is added so the output keeps the same size as the input.

6. Normalizing image pixels (0–255 → 0–1) Same purpose as StandardScaler in Week 6: keeps all input values on a small, consistent scale, which makes training faster and more stable — without it, large raw pixel values (up to 255) can destabilize training, similar to the exploding-gradient issue seen with a high learning rate in Week 6.

What I Did Today
Reviewed Sprint 1 (Week 6) results and confirmed no serious overfitting occurred — a small, stable gap between training and validation, with Dropout/BatchNorm added as a precaution that also improved test performance.
Completed Sprint 2 planning: the project shifts from the tabular medical dataset to a new image dataset (Melanoma Skin Cancer — Benign vs Malignant), since the data type calls for a CNN, not the Dense network used in Week 6.
Applied a hand-defined edge-detection filter (3×3) to a sample image using convolution, and visualized the resulting feature map — clearly saw the filter highlight the irregular border of the shape while ignoring flat, unchanging regions.
Documented in Markdown why parameter sharing makes a CNN need far fewer weights than an equivalent Dense layer.
Confirmed and recorded the architecture decision: CNN, since the project now works with image data.
Wrote the Sprint 2 backlog (data prep, EDA on images, new image baseline, CNN build, training/evaluation, tuning, documentation).
Key Takeaway

A CNN is not "a better neural network" in general — it's the right tool specifically because the data is images with spatial structure. The Week 6 Logistic Regression baseline (0.792 accuracy) does not carry over to this new image-based task; a new baseline needs to be established before the CNN's improvement can be measured fairly.
"""
