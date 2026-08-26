# Day 4 Summary — Neural Networks with Keras

## What I Learned

Today I built my first neural network using **TensorFlow/Keras** for the binary classification task on my medical dataset (predicting `Result`: positive/negative).

**1. Building a model**
I used the Keras `Sequential` API to stack `Dense` layers. Each `Dense` layer connects every input to every neuron. My final layer used `sigmoid` activation because this is a binary classification problem — it outputs a probability between 0 and 1.

**2. Compile → Fit → Evaluate**
- `compile()` sets the optimizer (`adam`), loss (`binary_crossentropy`), and metric (`accuracy`).
- `fit()` trains the model over epochs, using a separate validation set to monitor performance on unseen data.
- `evaluate()` gives the final, honest score on the test set.

**3. Diagnosing the fit**
I plotted training vs. validation loss/accuracy from the `history` object. The first model (no regularization) showed a small, stable gap between training and validation — a healthy fit, with only a mild early hint of overfitting.

**4. Dropout & Batch Normalization**
Adding `Dropout(0.3)` and `BatchNormalization()` changed the training curves — validation metrics looked *better* than training metrics. I learned this is expected: Dropout is active during training (weakening the network on purpose) but turned off during validation/evaluation, so the comparison during training isn't apples-to-apples. The real, fair comparison only happens at `evaluate()` time, where Dropout is off for both.

**5. Comparing to the baseline**
| Model | Test Accuracy |
|---|---|
| Logistic Regression (baseline) | 0.792 |
| NN v1 (no regularization) | 0.793 |
| NN v2 (Dropout + BatchNorm) | **0.889** |

**Key takeaway:** A plain neural network didn't beat the classical baseline on this small dataset (~1,300 rows, 8 features) — deep learning isn't automatically better. The real improvement came from regularization (Dropout + BatchNorm), which helped the model generalize instead of just fitting the training data.

## Tools Used
TensorFlow / Keras · Matplotlib · Scikit-learn (baseline) · Jupyter/VS Code
