---
layout: two-cols
layoutClass: "gap-8 grid-cols-[0.42fr_0.58fr]"
---

![A test-taking process represented as a complex clickstream](/images/test-taking-process.png){width=88%}

::right::

<v-clicks depth="1" class="text-sm leading-5">

- **Supervised outcome prediction**  
  Use process data to predict accuracy, scores, completion, or other external outcomes.
  *Examples: [Chen et al. (2019)](https://doi.org/10.3389/fpsyg.2019.00486); [Chen, Lu, & Cui (2024)](https://doi.org/10.1007/s10639-023-12389-x).*

- **Unsupervised behavioral-pattern exploration**  
  Uncovering recurring states, test taking strategies, or response patterns within the process data.
  *Examples: [Ulitzsch, He, & Pohl (2022)](https://doi.org/10.3102/10769986211010467); [He, Borgonovi, & Paccagnella (2021)](https://doi.org/10.1016/j.compedu.2021.104170).*

</v-clicks>

<div v-click class="mt-5 text-[0.58rem] leading-4">

| student_id | seq_len | event_sequence |
|---|---:|---|
| S001 | 5 | `Q1:view@0, Q1:answer@12, Q2:view@15, Q3:view@41, Q3:submit@45` |
| S002 | 11 | `Q1:view@0, Q2:view@3, Q1:view@9, Q1:view@9, ... Q4:submit@140` |
| S003 | 3 | `Q1:view@0, Q1:idle@300, Q1:submit@301` |
| ... | ...| `Q1:view@0, null@2` |

</div>

---

# Four Broad Strategies for Representing Clickstreams

Conventional statistical analyses generally require each respondent to have the same set of dimensions.

1. **Engineered features**  
   Summarize time, action counts, transitions, n-grams, or task-specific behaviors.

2. **Sequence similarity or distance**  
   Compare whole sequences using edit distance, longest common subsequence, or optimal matching; analyze the resulting distances directly or through MDS.

3. **Latent process representations**  
   Describe behavior using a bounded set of latent states, classes, or transition parameters.

4. **Learned sequence representations**  
   Use autoencoders, RNNs, or transformers to learn fixed-dimensional embeddings.

> **The gap is not a lack of ways to reduce the data. The challenge is learning representations that preserve contextual detail while remaining reusable across analytic goals.**

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
