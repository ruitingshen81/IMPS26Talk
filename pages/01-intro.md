---
layout: two-cols
layoutClass: "gap-8 grid-cols-[0.42fr_0.58fr]"
---

![A test-taking process represented as a complex clickstream](/images/test-taking-process.png){width=88%}

::right::

<v-clicks depth="1" class="text-sm leading-5">

- **Supervised outcome prediction**  
  Use process data to predict accuracy, completion efficiency, or other external outcomes.
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

# Three Broad Strategies for Representing Clickstreams

Conventional statistical analyses generally require each respondent to have the same set of dimensions.

1. **Engineered summary features**  
   Summarize time, action counts, transitions, n-grams, or task-specific behaviors.

2. **Sequence similarity using distance matrix**  
   Compare whole sequences using edit distance, longest common subsequence, or optimal matching; analyze the resulting distances directly or through MDS.

3. **Learned sequence representations from sequence models**  
   Use autoencoders, RNNs, or LSTMs to learn fixed-dimensional embeddings.

> <span style="font-size: 1.5rem; line-height: 1.45;">
>   <span class="font-bold text-[#9f331d]">The gap:</span>
>   using a learned model does not automatically solve the representation problem. The remaining challenge is to learn embeddings that preserve contextual information from the full sequence while also being reusable across different analytic goals.
> </span>

---
layout: center
---

# Where This Study Sits

<v-switch>
<template #1>

```mermaid {theme: 'neutral', scale: 0.78}
flowchart LR
  Raw["Raw clickstream<br/>actions + times"]

  Sup["Supervised outcome<br/>prediction"]
  Feat["Feature compression<br/>and representations"]
  Unsup["Unsupervised behavioral-pattern<br/>exploration"]

  Rep["Sequence representations"]

  Raw --> Feat
  Feat --> Sup
  Feat --> Unsup
  Feat ~~~ Rep
  Rep --> Feat

  classDef raw fill:#1f6f78,color:#fff,stroke:#164f56,stroke-width:2px;
  classDef approach fill:#f5f2e8,color:#27231d,stroke:#d6cdb9,stroke-width:2px;
  classDef rep fill:#1f6f78,color:#fff,stroke:#164f56,stroke-width:2px;
  class Raw raw;
  class Sup,Unsup,Feat approach;
  class Rep rep;
```

</template>
<template #2>

```mermaid {theme: 'neutral', scale: 0.78}
flowchart LR
  Raw["Raw clickstream<br/>actions + times"]

  Sup["Supervised outcome<br/>prediction"]
  Unsup["Unsupervised behavioral-pattern<br/>exploration"]
  Feat["Feature compression<br/>and representations"]
  Trans["Contextual sequence embedding <br/> from transformer model"]

  Rep["Sequence representations<br/>(embeddings)"]

  Raw --> Feat
  Feat --> Sup
  Feat --> Unsup
  Trans -- "expands" --> Rep
  Feat ~~~ Rep
  Rep --> Feat

  classDef raw fill:#1f6f78,color:#fff,stroke:#164f56,stroke-width:2px;
  classDef grey fill:#e4e4e4,color:#777,stroke:#b8b8b8,stroke-width:2px;
  classDef feature fill:#f5f2e8,color:#27231d,stroke:#d6cdb9,stroke-width:2px;
  classDef transformer fill:#d94f30,color:#fff,stroke:#9f331d,stroke-width:3px,font-size:22px,font-weight:700;
  classDef rep fill:#1f6f78,color:#fff,stroke:#164f56,stroke-width:2px;
  class Raw raw;
  class Sup,Unsup grey;
  class Feat feature;
  class Trans transformer;
  class Rep rep;
```

</template>
<template #3>

```mermaid {theme: 'neutral', scale: 0.72}
flowchart LR
  Raw["Raw clickstream<br/>actions + times"]

  Sup["Supervised outcome<br/>prediction"]
  Unsup["Unsupervised behavioral-pattern<br/>exploration"]
  Feat["Feature compression<br/>and representations"]
  Trans["Contextual sequence embedding <br/> from transformer model"]

  Rep["Sequence representations<br/>(embeddings)"]

  U1["Predict task<br/>outcomes"]
  U2["Cluster response<br/>strategies"]
  U3["Study process<br/>patterns"]
  U4["Compare tasks<br/>or populations"]

  Raw --> Feat
  Feat --> Sup
  Feat --> Unsup
  Trans -- "expands" --> Rep

  Feat ~~~ Rep
  Rep --> Feat
  Rep --> U1
  Rep --> U2
  Rep --> U3
  Rep --> U4

  classDef raw fill:#1f6f78,color:#fff,stroke:#164f56,stroke-width:2px;
  classDef grey fill:#e4e4e4,color:#777,stroke:#b8b8b8,stroke-width:2px;
  classDef feature fill:#f5f2e8,color:#27231d,stroke:#d6cdb9,stroke-width:2px;
  classDef transformer fill:#d94f30,color:#fff,stroke:#9f331d,stroke-width:3px,font-size:22px,font-weight:700;
  classDef rep fill:#1f6f78,color:#fff,stroke:#164f56,stroke-width:2px;
  classDef use fill:#fff7ed,color:#27231d,stroke:#d94f30,stroke-width:1.5px;
  class Raw raw;
  class Sup,Unsup grey;
  class Feat feature;
  class Trans transformer;
  class Rep rep;
  class U1,U2,U3,U4 use;
```

</template>
</v-switch>

---
layout: two-cols-header
layoutClass: gap-12
class: "text-[0.7em] leading-snug [&_h2]:!mb-2 [&_h2]:!text-[1.65em] [&_h3]:!mb-1 [&_h3]:!text-[1.15em] [&_li]:!my-1"
---

<div class="flex items-center gap-7">
  <h1 class="!m-0">Why Transformer?</h1>
  <v-click>
    <div class="rounded-md bg-[#d94f30] px-5 py-2 text-2xl font-bold text-white">
    Attention is all you need!
    <span class="ml-2 text-sm font-normal opacity-85">(Vaswani et al., 2017)</span>
</div>
  </v-click>
</div>

::left::

<v-click>

## RNNs and LSTMs

### Recurrent mechanism

- Process actions one at a time
- Update a hidden state that carries the previous history:

$$h_t=f(x_t,h_{t-1})$$

### Limitations for long clickstreams

- Distant evidence must pass through many recurrent updates
- One evolving hidden state can become an information bottleneck
- LSTM gates reduce but do not remove these challenges

</v-click>

::right::

<v-click>

## Self-attention

### Attention mechanism

1. Each action can directly evaluate which earlier actions are relevant
2. The model creates **query**, **key**, and **value** vectors for each token/action.
3. Query–key similarity determines the attention weights (which earlier actions matter):

$$\alpha_{ij}=\operatorname{softmax}\left(\frac{q_i k_j^\top}{\sqrt d}\right)$$

3. Their values are combined into a contextual representation:

$$z_i=\sum_{j\leq i}\alpha_{ij}v_j$$


</v-click>
