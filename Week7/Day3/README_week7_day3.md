# Week 7, Day 3 Summary — RNN, LSTM & Debugging a Real Model

## What I Worked On

Trained two sequence models — SimpleRNN and LSTM — on the PTBDB heartbeat dataset (ECG signals, Normal vs Abnormal), using `ptbdb_normal.csv` and `ptbdb_abnormal.csv`. Each row is a time series (188 points) representing one heartbeat, and the last column is the class label.

## What I Learned

**1. Why RNNs/LSTMs fit this data type**
Unlike the tabular data (Week 6) or images (Week 7 Day 1–2), ECG signals are sequences — each value depends on its position in time relative to the others. RNN-family models are built to process data in order, carrying information forward step by step, which fits a heartbeat signal naturally.

**2. SimpleRNN's core weakness — vanishing gradients**
The SimpleRNN's validation accuracy came out almost perfectly flat across training. Diagnosed this by checking the class distribution and finding the majority class made up about 72% of the data — matching the flat accuracy almost exactly. Conclusion: the model wasn't learning at all; it converged early to always predicting the majority class. This is a classic SimpleRNN weakness on longer sequences — gradients from early time steps shrink to nearly nothing by the time they reach the start of the sequence, so the network struggles to learn from long-range patterns and settles for a trivial shortcut.

**3. LSTM showed real learning, but unstable training**
LSTM's validation curves moved meaningfully (unlike SimpleRNN), but loss spiked sharply at one point during training — a sign of instability, not healthy convergence. Addressed this with two standard fixes: a lower learning rate (reduces the size of each weight update) and gradient clipping (`clipnorm`), which caps how large a single gradient update can be, preventing sudden destabilizing jumps — a technique particularly relevant to RNN/LSTM training.

**4. Confusion matrix as a diagnostic tool**
A single accuracy number can hide a model that's just guessing the majority class. A confusion matrix (and per-class precision/recall) makes this visible immediately — if one predicted-class column is empty, the model isn't actually discriminating between classes, no matter how good the accuracy looks.

**5. Proper train/val/test separation**
Learned to split data explicitly into three sets (not just rely on an automatic validation split), so a genuine, untouched test set exists for the final evaluation — same principle as Week 6, applied here with `stratify=y` to keep the same class proportions across all three sets given the imbalance found in this dataset.

## Key Takeaway

A flat or suspiciously "clean" accuracy curve is not automatically good news — it can mean a model gave up and is just predicting the majority class. Always check the class balance and look at a confusion matrix before trusting an accuracy number, especially on imbalanced data.
