# Day 2 — From Text to Numbers (AG News)

## Goal
Text data must be converted into numeric vectors before any model can use
it. Day 2 covers the two main families of approaches — count-based
(TF-IDF) and meaning-based (word embeddings) — and builds a hands-on
comparison of both against deep learning models, using the AG News topic
classification dataset (4 classes: World, Sports, Business, Sci/Tech).

## Concepts Covered

### Bag-of-Words & TF-IDF
Represents a document by which words it contains and how often, ignoring
order. TF-IDF improves on raw counts by weighting each word: high if
frequent in one document (TF) but rare across all documents (IDF) — this
down-weights common words and surfaces distinctive ones. Simple, fast,
and a strong baseline for text classification.

### Word Embeddings (Word2Vec / GloVe)
Represents each word as a dense vector positioned so that similar-meaning
words sit close together in space — capturing semantic relationships that
TF-IDF cannot (e.g. `king - man + woman ≈ queen`). Limitation: each word
gets exactly one fixed vector, regardless of context.

### Contextual Embeddings (Transformers / BERT)
Produce a different vector for the same word depending on its sentence —
resolving ambiguity (e.g. "bank" as river vs. money) that static
embeddings can't handle. This is why transformer-based models outperform
older approaches on nuanced language tasks.

## Hands-On Lab — What Was Done

### Step 1: TF-IDF Baseline
- Cleaned AG News text (regex-based punctuation stripping, stopword
  removal with negation words preserved, lemmatization).
- Vectorized with `TfidfVectorizer(max_features=5000)`.
- Trained Logistic Regression on a fixed 5,000-example subsample.
- **Result: 87.76% accuracy, 1.0 second training time.**

### Step 2: Word Embeddings — Nearest Neighbors
- Loaded pre-trained `glove-wiki-gigaword-100` vectors.
- Queried nearest neighbors for domain-relevant words:
  - `king` → prince, queen, monarch
  - `computer` → software, hardware, pc, technology
  - `stock` → shares, market, trading
  - `team` → squad, players, coach
  - `war` → conflict, invasion, military
- Confirmed each cluster maps cleanly onto one of AG News's 4 categories,
  showing embeddings capture topic-relevant meaning without ever seeing
  the dataset's labels.

### Step 3: Model Comparison
Compared TF-IDF against an LSTM (trained from scratch) and a transformer,
all evaluated on the same 5,000-example training budget where applicable:

| Model | Accuracy | Training Required | Time |
|---|---|---|---|
| Transformer (zero-shot, bart-large-mnli) | 70.50% | None | 30 sec |
| LSTM (trained from scratch) | 80.00% | Yes (5 epochs) | ~1 min |
| TF-IDF + Logistic Regression | 87.76% | Yes (1 pass) | 1.0 sec |
| DistilBERT (fine-tuned) | 90.83% | Yes (3 epochs) | few min (GPU) |

*Note: the zero-shot transformer never trained on the dataset at all, so
it isn't directly comparable to the trained models — it's included to
show the trade-off between needing zero labeled data vs. needing some.*

### Step 4: Documentation — Which Representation Fits This Project?
**TF-IDF + Logistic Regression is the recommended default** for this
task: it reached 87.76% accuracy in ~1 second, within a few points of the
much more expensive fine-tuned transformer (90.83%, minutes on a GPU).
The LSTM underperformed both, showing that training a sequence model from
scratch on only 5,000 examples isn't enough data for it to compete with
either a strong classical baseline or a pre-trained model. Fine-tuned
transformers remain the accuracy ceiling when GPU time is available and
the extra points matter.

## Key Takeaways
- TF-IDF is fast, interpretable, and a genuinely strong first choice —
  don't skip straight to deep learning by default.
- Embeddings capture meaning TF-IDF cannot, but static embeddings (GloVe,
  Word2Vec) still assign one fixed vector per word regardless of context.
- Contextual embeddings (BERT-family models) resolve that ambiguity and,
  when fine-tuned on task-specific data, deliver the best accuracy —
  at the cost of more training time and compute.
- Representation choice should be driven by constraints (data available,
  time, compute), not accuracy alone.

## Files/Artifacts
- Cleaned train/test datasets (negation-preserving tokenization)
- GloVe nearest-neighbor exploration outputs
- TF-IDF + Logistic Regression model and classification report
- Final 4-model comparison table (this document)
