---
layout: section
---

# Results
Sequence level, token level

---
layout: two-cols-header
layoutClass: gap-2
---

# Sequence-level: cluster solution

::left::

![Silhouette analysis for mean-pooled sequence embeddings](/images/silhouette_mean_pooled.png){width=85%}

::right::

![Silhouette analysis for last-token sequence embeddings](/images/silhouette_last_token.png) {width=85%}



---
layout: two-cols-header
layoutClass: gap-2
---

# Sequence-level: Clusters on first 2PC

::left::

![Clusters on first 2PC - Mean pooled](/images/mean_pooled2pc.png){width=80%}

::right::

![Clusters on first 2PC - Last token](/images/last_token2pc.png){width=80%}





---
layout: two-cols-header
layoutClass: gap-2
---

# Sequence clusters using mean-pooled embeddings

::left::

![Sequences in cluster 1](/images/seq1_mean_pooled.png){width=80%}

::right::

![Sequences in cluster 2](/images/seq2_mean_pooled.png){width=80%}


---
layout: two-cols-header
layoutClass: gap-2
---

# Sequence clusters using last token embeddings

::left::

![Sequences in cluster 1](/images/combined_seq_last_token1.png){width=90%}


::right::

![Sequences in cluster 2](/images/combined_seq_last_token2.png){width=90%}



---
layout: two-cols-header
layoutClass: gap-2
---

# Token level: Token distance in the vector space

::left::

![token distance](/images/token_embedding_distance_heatmap.png){width=125%}


::right::

- **Scratchwork actions form the clearest neighborhood:** Clear Scratchwork (`CS`) is close to Scratchwork Draw Mode On (`SDO`), Draw (`D`), and Scratchwork Mode On (`SON`).
- **Other close pairs reflect process transitions:** Exit Item–Next (`EXI`–`N`) and Click Progress Navigator–Enter Item (`CPN`–`ENI`).
- **The nearest pair is First Text Change–Math Keypress** (`FTC`–`MK`).
- Overall, the embeddings capture **shared sequence context**, not only literal action categories.
