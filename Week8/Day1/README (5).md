# Sprint 3 — Text Preprocessing Notes

## What this covers

Today's work focused on building and validating a text cleaning pipeline for the sentiment analysis task, tested against the tiny-shakespeare dataset.

## What I understood and did

**Tokenization**

I used `word_tokenize` instead of a simple `.split()` because it correctly separates contractions into their real grammatical pieces — for example, `"wasn't"` becomes `"was"` + `"n't"`, not one messy blob. This matters because if contractions stay stuck together, the cleaning steps later (like stopword removal) can't work on them properly.

**The cleaning pipeline**

The full pipeline runs in this order:
1. Lowercase everything
2. Tokenize
3. Remove punctuation
4. Remove stop words
5. Lemmatize (reduce words to their base form, e.g. "running" → "run")

Punctuation is removed *after* tokenization, not before, so the tokenizer can still use punctuation to correctly find sentence and word boundaries first.

**The negation problem (the most important thing I learned)**

Standard stopword lists remove words like "not", "no", "never", and the tokenized fragment `"n't"`. That's a problem for sentiment tasks, because those words can flip the entire meaning of a sentence — "not good" and "good" are opposites, but a naive pipeline would treat them the same after cleaning.

To fix this, I pulled negation-related words out of the stopword list before filtering, so they survive the cleaning process. I verified this was working correctly by explicitly checking that words like "not", "n't", "no", and "nor" still appeared in the cleaned output.

**Testing on a real dataset**

I ran the pipeline on the tiny-shakespeare dataset instead of just toy sentences. This was useful because Shakespeare's text has older phrasing and contractions (like "know'st not") that a modern pipeline might not expect — a good stress test for whether the negation-preservation logic actually holds up on messier, real-world text, not just clean examples I wrote myself.

## Why this matters for the project

If negation words get silently stripped during preprocessing, any sentiment model trained on the cleaned text would lose the ability to detect negated statements — a core failure mode for sentiment analysis. Verifying and documenting this decision now avoids a hard-to-diagnose bug later.

## Open items

- Sprint 3 backlog selection for integration and evaluation tasks (team decision, not yet finalized)
- Could still check for other archaic contraction forms in tiny-shakespeare (e.g. "'tis", "o'er") that the tokenizer might not split as expected
