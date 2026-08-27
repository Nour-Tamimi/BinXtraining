# Day 5 Summary — Tuning, Callbacks & Sprint 1 Close-Out

## What I Learned

**1. Hyperparameter tuning strategy**
The highest-impact knobs to tune, in priority order: learning rate, network width/depth, dropout rate, and batch size. The right approach is to change **one variable at a time** and watch the validation loss curve — the same disciplined method used for classical model tuning in Week 4.

**2. Callbacks automate good training habits**
- **EarlyStopping** watches the validation loss and stops training once it stops improving for a set number of epochs (patience). This avoids wasting time on epochs that don't help, and reduces the risk of overfitting from training too long.
- **restore_best_weights** ensures the model keeps the *best* version it saw during training, not just whatever the last epoch happened to produce.
- **ModelCheckpoint** saves the best model to disk during training, so it can be reloaded later even in a new session.

**3. Sprint discipline**
Every result needs evidence before it can be demoed: loss curves, a metrics table comparing models, and a written narrative — not just a final number. Acceptance criteria for this sprint included: the notebook runs error-free, work is committed to the right branch, the pull request is mentor-approved, and results are documented and compared against the baseline.

**4. Sprint Review vs. Retrospective**
A Sprint Review demos completed work to the mentor. A Retrospective is a separate, honest look back — what went well, what to improve, and one concrete change to carry into the next sprint.

## What I Accomplished Today

- Ran four controlled hyperparameter experiments (higher dropout, lower learning rate, higher learning rate, smaller network), changing one variable at a time and logging each result.
- Found that a higher learning rate (0.005) combined with Dropout + BatchNormalization gave the best validation performance among the experiments (94% val accuracy).
- Built a final tuned model with EarlyStopping, which automatically stopped training at epoch 28 instead of running the full 100 — saving significant training time with no real loss in performance.
- Evaluated the final model on the untouched test set, closing out Sprint 1 with a full comparison table against the Logistic Regression baseline and the earlier neural network versions.

## Key Takeaway

Every regularized neural network beat the classical baseline by 8–10 accuracy points, but fine-tuning the learning rate on top of Dropout/BatchNorm gave no further major gain — the real improvement came from regularization itself, not from micro-tuning. EarlyStopping proved genuinely useful in practice, not just in theory.
