# Day 4: Transformers & Attention — What I Learned

## 1. Why RNNs/LSTMs Fall Short
LSTMs process text one word at a time, carrying a single memory state forward.
This makes them **slow** (no parallelism) and **weak on long-range context**,
since early information can fade as the sequence grows.

## 2. The Attention Mechanism
Attention lets every word look directly at every other word in the sequence
and decide how relevant it is, instead of relying on a step-by-step memory.

- **Self-attention**: each element weighs the relevance of every other element
- **Parallelism**: all positions processed at once → much faster training
- **Long-range context**: direct access to distant words, no fading memory

## 3. The Transformer Architecture
Transformers stack attention layers with feed-forward layers. Since attention
has no built-in sense of word order, **positional encoding** is added to give
the model that information explicitly.

## 4. Pre-trained Transformers & Hugging Face
Just like reusing a pre-trained CNN for images, pre-trained Transformers
(BERT, DistilBERT, GPT-2, BART) can be reused for text tasks with minimal
code via the Hugging Face `pipeline`.

## 5. Hands-On Results (AG News dataset)

| Model | Accuracy | Training Required | Time |
|---|---|---|---|
| Transformer (zero-shot, bart-large-mnli) | 70.50% | None | 21.7 seconds |
| LSTM (trained from scratch, 5000 examples) | 80.00% | Yes (5 epochs, early stopping) | ~1 minute |

**Key takeaway:** the Transformer scored lower here only because it was used
*zero-shot* — it never saw a single AG News example. The LSTM won because it
was trained directly on the task. A **fine-tuned** Transformer would likely
beat the LSTM.

## 6. Two Bugs I Hit and Fixed
- **Frozen validation accuracy**: caused by an unshuffled, label-sorted
  dataset split — fixed by shuffling before sampling.
- **Stuck loss at random-guess level**: caused by the LSTM processing padding
  tokens as real input — fixed with `mask_zero=True`.

## 7. Chosen Architecture for the Project
**LSTM** — it achieved higher accuracy (80.00% vs. 70.50%) after training on
task-specific data. A fine-tuned Transformer remains the stronger long-term
option if more time/resources are available.
