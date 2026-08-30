# Week 6 Summary — Neural Networks (Keras)

## Day 1 — Baseline Model
Built a Logistic Regression baseline on the project's medical dataset (heart disease prediction). This baseline score is the number every later model must beat.

**Result:** Accuracy 0.792, Precision 0.828, Recall 0.833, F1 0.831, ROC-AUC 0.889.
**Key decision:** Recall/F1 matter more than raw accuracy here, since missing a real heart-disease case (false negative) is more costly than a false alarm.

## Day 2 — Activation Functions & Forward Pass
Compared Sigmoid, Tanh, and ReLU to understand how each transforms values. Decided on ReLU for hidden layers and Sigmoid for the output layer (binary classification), with Binary Cross-Entropy as the loss — the exact setup used for the rest of the week. Computed one forward pass by hand with NumPy to understand what a Dense layer actually computes (weighted sum + activation) before letting Keras automate it.

## Day 3 — Training Loop & Backpropagation
Learned and implemented the four-step training loop from scratch in NumPy: **Forward Propagation** (produce a prediction), **Loss** (measure the error), **Backpropagation** (use the chain rule to find each weight's contribution to the error), and **Update** (adjust weights via Gradient Descent). Applied this manual implementation directly to the project's 8 medical features, using the same starting weights across learning-rate experiments for a fair comparison. Directly observed what happens when the learning rate is too high (exploding values/instability).

## Day 4 — First Keras Neural Network
Built the project's first real neural network using Keras: `Dense(64) → Dense(32) → Dense(1, sigmoid)`. Trained and evaluated it, then added Dropout and Batch Normalization to fight overfitting and stabilize training.

**Result:** Plain network (NN v1): 0.793 test accuracy — barely beat the baseline. With Dropout + BatchNorm (NN v2): 0.889 test accuracy — a clear, meaningful improvement.

## Day 5 — Tuning, EarlyStopping & Sprint 1 Close-Out
Ran controlled hyperparameter experiments (dropout rate, learning rate, network size), changing one variable at a time. Added `EarlyStopping` to stop training automatically once validation loss stopped improving, instead of guessing a fixed number of epochs.

**Result:** Training stopped automatically at epoch 28 (instead of 100), with 0.874 final test accuracy. Closed out Sprint 1 with a full comparison table against the baseline and earlier models, then wrote an honest retrospective identifying git branching discipline as the one concrete change for Sprint 2.

## Overall Takeaway for the Project
Every regularized neural network beat the classical baseline by 8–10 accuracy points, but the improvement came from **regularization (Dropout + BatchNorm)**, not from network depth or fine-grained hyperparameter tuning. This is now the strongest model for the project so far, with a clear, documented path (feature selection, class balancing, cross-validation, threshold tuning) for further improvement in later sprints.
