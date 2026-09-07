# Day 5 — Optimizing the Text Classification Project (AG News)

## Goal
Day 4 established a baseline comparison between an LSTM (trained from
scratch) and a zero-shot transformer. That comparison wasn't fully fair,
since the zero-shot transformer never got to train on the dataset at all.
Day 5's goal was to close that gap by fine-tuning a transformer on the
same data budget as the other models, and to bring in TF-IDF as an
additional, lightweight baseline — giving a complete, four-way comparison
of representation strategies for text classification.

## Dataset
**AG News** (`fancyzhx/ag_news`) — 4 balanced topic classes:
`World`, `Sports`, `Business`, `Sci/Tech`. 120,000 training articles,
7,600 test articles. For fair comparison across all models, training was
done on a fixed random subsample of **5,000 examples** (same `random.seed(42)`
indices reused across TF-IDF, LSTM, and DistilBERT).

## Work Done Today

### 1. Re-cleaned text pipeline (negation-preserving)
Rebuilt the Day 1 cleaning pipeline for this dataset:
- Lowercased text, stripped punctuation via regex (fixed an earlier bug
  where embedded punctuation like `high-tech` or HTML-entity leftovers
  like `#39;` weren't being removed by a plain `string.punctuation` filter).
- Removed stopwords, **except** negation words (`not`, `no`, `never`,
  contractions like `don't`/`won't`), since these carry meaning that
  matters for downstream classification.
- Lemmatized remaining tokens.
- Applied identically to both `train` and `test` splits.

### 2. Word embeddings (GloVe) — nearest neighbor exploration
Loaded pre-trained `glove-wiki-gigaword-100` vectors and inspected nearest
neighbors for domain-relevant words (`king`, `computer`, `stock`, `team`,
`war`). Confirmed that embeddings cluster words by *meaning*, not just
spelling — e.g., `stock` → `shares`, `market`, `trading`; `team` →
`squad`, `players`, `coach`. Each cluster mapped cleanly onto one of the
4 AG News categories, showing embeddings capture topic-relevant semantic
structure without ever seeing the dataset's labels.

### 3. Four-way model comparison
Ran and compared four representation/model strategies on the **same
5,000-example training budget** (or zero-shot, where no training budget
applies):

| Model | Accuracy | Training Required | Time |
|---|---|---|---|
| Transformer (zero-shot, bart-large-mnli) | 70.50% | None | 30 sec |
| LSTM (trained from scratch) | 80.00% | Yes (5 epochs) | ~1 min |
| TF-IDF + Logistic Regression | 87.76% | Yes (1 pass) | 1.0 sec |
| DistilBERT (fine-tuned) | 90.83% | Yes (3 epochs) | few min (GPU) |

### 4. Environment issue resolved
Fine-tuning DistilBERT initially failed with an `ImportError` from a
`torchvision`/`fastai` version conflict in Colab's default environment,
triggered by `.set_format("torch", ...)`. Fixed by removing that call and
using `DataCollatorWithPadding` instead, which converts batches to tensors
without touching the broken torchvision code path.

## Key Takeaways
- **Zero-shot vs. fine-tuned is not an apples-to-apples comparison** —
  zero-shot trades accuracy for needing zero labeled data; fine-tuning
  needs labeled data but specializes the model to the task.
- **TF-IDF is a remarkably strong, cheap baseline** — 87.76% accuracy in
  1 second, only ~3 points behind the fine-tuned transformer which took
  minutes on a GPU.
- **The LSTM underperformed both simpler and more sophisticated
  approaches** — training a sequence model from scratch on only 5,000
  examples isn't enough data for it to compete with either a strong
  classical baseline or a pre-trained model.
- **Recommendation for this project:** TF-IDF + Logistic Regression as the
  practical default; fine-tuned DistilBERT as the accuracy ceiling when
  GPU time is available.

## Files/Artifacts
- Cleaned train/test datasets (tokenized, negation-preserving)
- GloVe nearest-neighbor exploration outputs
- TF-IDF + Logistic Regression model and classification report
- Fine-tuned DistilBERT model, training log, and classification report
- Final 4-model comparison table (this document)
