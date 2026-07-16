---
layout: two-cols-header
layoutClass: gap-10
class: text-sm
---

# Future directions: What do the embeddings capture?

::left::

## Model extensions

- **Compare encoder and decoder representations.** Train a bidirectional encoder using masked-action prediction, then compare its token- and sequence-level embeddings with those from the causal decoder.
- **Separate the effects of training objective and pooling.** Compare masked-token versus next-token prediction and mean pooling versus special-token or last-token representations.
- **Test generalizability.** Repeat the analysis across items, time-window sizes, and rules for selecting actions within each window.

::right::

## Validation and interpretation

- **Establish external validity.** Relate embeddings and clusters to response accuracy, completion time, scores, and known response strategies.
- **Evaluate stability.** Check whether token neighborhoods and sequence clusters persist across random seeds, model settings, and bootstrap samples.
- **Interpret behavioral patterns.** Examine representative sequences, nearest neighbors, and influential time steps to identify what distinguishes each cluster.
