---
layout: default
---

# Existing Approaches

<v-clicks depth="2">

1. **Supervised sequence prediction**
   - Learn sequence representations optimized for predicting task outcomes.
   - Examples: RNNs, LSTMs, GRUs over action and time sequences.

2. **Latent process-state models**
   - Represent clickstream behavior as transitions among discrete latent states.
   - Examples: HMMs, latent transition models, sequence-oriented latent classes.

3. **Feature extraction and sequence compression**
   - Convert irregular clickstreams into fixed-dimensional summaries.
   - Examples: handcrafted process features, OSS/MDS features, RNN autoencoders.

</v-clicks>

---

# The Gap

<v-clicks>

- Clickstream sequences are irregular, high-dimensional, and context dependent.
- Existing methods often summarize behavior into discrete states, handcrafted features, or prediction-targeted representations.
- These approaches can lose sequence-wide contextual information.
- We need a representation-learning approach that preserves action-level context while producing scalable respondent-level embeddings.

</v-clicks>


---
layout: center
---

# Where This Study Sits

<v-switch>
<template #1>

```mermaid {theme: 'neutral', scale: 0.78}
flowchart LR
  Raw["Raw clickstream<br/>actions + times"]

  Sup["Supervised sequence<br/>prediction"]
  Lat["Latent process-state<br/>models"]
  Feat["Feature extraction<br/>and compression"]

  Rep["Sequence representations"]

  Raw --> Sup
  Raw --> Lat
  Raw --> Feat
  Feat --> Rep

  classDef raw fill:#1f6f78,color:#fff,stroke:#164f56,stroke-width:2px;
  classDef approach fill:#f5f2e8,color:#27231d,stroke:#d6cdb9,stroke-width:2px;
  classDef rep fill:#1f6f78,color:#fff,stroke:#164f56,stroke-width:2px;
  class Raw raw;
  class Sup,Lat,Feat approach;
  class Rep rep;
```

</template>
<template #2>

```mermaid {theme: 'neutral', scale: 0.78}
flowchart LR
  Raw["Raw clickstream<br/>actions"]

  Sup["Supervised sequence<br/>prediction"]
  Lat["Latent process-state<br/>models"]
  Feat["Feature extraction<br/>and compression"]
  Trans["Contextual sequence embedding <br/> from transformer model"]

  Rep["Sequence representations<br/>(embeddings)"]

  Raw --> Sup
  Raw --> Lat
  Raw --> Feat
  Feat -- "expands" --> Trans
  Feat --> Rep
  Trans ==> Rep

  classDef raw fill:#1f6f78,color:#fff,stroke:#164f56,stroke-width:2px;
  classDef grey fill:#e4e4e4,color:#777,stroke:#b8b8b8,stroke-width:2px;
  classDef feature fill:#f5f2e8,color:#27231d,stroke:#d6cdb9,stroke-width:2px;
  classDef transformer fill:#d94f30,color:#fff,stroke:#9f331d,stroke-width:3px;
  classDef rep fill:#1f6f78,color:#fff,stroke:#164f56,stroke-width:2px;
  class Raw raw;
  class Sup,Lat grey;
  class Feat feature;
  class Trans transformer;
  class Rep rep;
```

</template>
<template #3>

```mermaid {theme: 'neutral', scale: 0.72}
flowchart LR
  Raw["Raw clickstream<br/>actions + times"]

  Sup["Supervised sequence<br/>prediction"]
  Lat["Latent process-state<br/>models"]
  Feat["Feature extraction<br/>and compression"]
  Trans["Transformer<br/>contextual sequence embedding"]

  Rep["Sequence representations<br/>(embeddings)"]

  U1["Predict task<br/>outcomes"]
  U2["Cluster response<br/>strategies"]
  U3["Study process<br/>patterns"]
  U4["Compare tasks<br/>or populations"]

  Raw --> Sup
  Raw --> Lat
  Raw --> Feat
  Feat -- "expands" --> Trans
  Feat --> Rep
  Trans ==> Rep
  Rep --> U1
  Rep --> U2
  Rep --> U3
  Rep --> U4

  classDef raw fill:#1f6f78,color:#fff,stroke:#164f56,stroke-width:2px;
  classDef grey fill:#e4e4e4,color:#777,stroke:#b8b8b8,stroke-width:2px;
  classDef feature fill:#f5f2e8,color:#27231d,stroke:#d6cdb9,stroke-width:2px;
  classDef transformer fill:#d94f30,color:#fff,stroke:#9f331d,stroke-width:3px;
  classDef rep fill:#1f6f78,color:#fff,stroke:#164f56,stroke-width:2px;
  classDef use fill:#fff7ed,color:#27231d,stroke:#d94f30,stroke-width:1.5px;
  class Raw raw;
  class Sup,Lat grey;
  class Feat feature;
  class Trans transformer;
  class Rep rep;
  class U1,U2,U3,U4 use;
```

</template>
</v-switch>

---
layout: two-cols
layoutClass: gap-16
---

# Why Using a Transformer?

- **Sequential context**
  - Masked self-attention lets each action condition on previous actions

- **Flexible positions**
  - Action, position, and time features can be embedded together before the transformer layers

- **Reusable embeddings**
  - The final sequence representation can support prediction, clustering, and interpretation

::right::

```mermaid {theme: 'neutral', scale: 0.84}
flowchart TB
  A1["Open item"] --> A2["Draw"]
  A2 --> A3["Revise"]
  A3 --> A4["Submit"]

  A1 -. supports .-> A2
  A1 -. supports .-> A3
  A2 -. supports .-> A3
  A1 -. supports .-> A4
  A2 -. supports .-> A4
  A3 -. supports .-> A4

  classDef action fill:#f5f2e8,color:#27231d,stroke:#d6cdb9;
  classDef final fill:#1f6f78,color:#fff,stroke:#164f56;
  class A1,A2,A3 action;
  class A4 final;
```
