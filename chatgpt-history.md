

KV cache is _just_ a way to **remember the past computations in attention so you don’t redo them every time you generate a new token**. That’s it. Everything else is detail.

I’ll go top-down (Minto style):

1. **Key message**
    
2. **Why KV cache is needed**
    
3. **How attention works (with math)**
    
4. **What exactly is cached and how**
    
5. **Tradeoffs & edge cases**
    

After each section I’ll ask you a few _first-principles_ questions you can try to answer.

---

## 1. Key message (top of the pyramid)

**Key idea:**  
In an autoregressive transformer, at time step (t), the new token needs to attend to **all tokens 1…t**. That means:

- We need **keys** (K_1, \dots, K_t)
    
- And **values** (V_1, \dots, V_t)
    

Computing those for all previous tokens _again_ at every step is wasteful.

> **KV cache** means:
> 
> - When token (i) is processed the first time, we compute its **key** (K_i) and **value** (V_i) **once** and store them.
>     
> - At later steps, for token (t), we **only** compute the new query (Q_t) and reuse the stored (K_1..K_{t-1}, V_1..V_{t-1}).
>     

So:

- **Time saved:** no recompute for old tokens
    
- **Cost:** extra **memory** to store all (K) and (V) for every layer and head.
    

---

### ❓ Questions (top-level intuition)

1. In your own words:
    
    > _Why does an autoregressive model even need information about all previous tokens when generating the next token?_
    
2. Suppose you **don’t** use a KV cache. If the sequence length is (T), how many times is the **first token’s** key (K_1) recomputed during generation of the full sequence?
    
3. From a systems perspective:
    
    > _What are we trading off when we decide to “cache” intermediate results instead of recomputing them?_
    

---

## 2. Why KV cache is needed (from first principles)

Let’s reason from scratch about **naive autoregressive decoding**.

You have a transformer decoder-only LM. For a given sequence of (T) tokens, and one attention layer:

- At step (t), the model takes tokens (x_1, \dots, x_t) and computes:
    
    - Their queries (Q_1, \dots, Q_t)
        
    - Their keys (K_1, \dots, K_t)
        
    - Their values (V_1, \dots, V_t)
        
- Then it uses these to compute the output for token (t).
    

In _batch generation_ mode, you might feed the whole sequence at once (training or prompt processing). But in **autoregressive inference**:

- At step 1, you compute for tokens ([x_1])
    
- At step 2, you compute for tokens ([x_1, x_2])
    
- At step 3, for ([x_1, x_2, x_3])
    
- … and so on.
    

**Naively**, at step (t), you recompute keys and values for _all_ (t) tokens. That means:

[  
\text{Total work over full sequence} \sim 1 + 2 + 3 + \dots + T = O(T^2)  
]

But notice something important:

> The hidden state of **past tokens doesn’t change** in pure autoregressive decoding.  
> So their keys and values **don’t change either**.

That’s the core first-principles argument for caching.

---

### ❓ Questions (complexity thinking)

4. Derive this yourself:  
    If at step (t) you process (t) tokens (recomputing everything), and you go from (t=1) to (T), show why total work is proportional to (T^2).
    
5. Now imagine we **cache** (K) and (V) for previous tokens, and at each step we only compute (K_t, V_t, Q_t) for the new token.
    
    - Roughly, what does the time complexity become in terms of (T)?
        
6. Why can we safely assume that old (K_i, V_i) don’t need to be recomputed at each step during pure decoding?
    

---

## 3. How self-attention works (with math)

Now let’s go down a level and formalize attention. Consider one attention head for simplicity.

### 3.1. From token embeddings to Q, K, V

Let:

- (x_t \in \mathbb{R}^{d_{\text{model}}}) be the embedding/hidden state of token (t).
    
- Learnable matrices:
    
    - (W_Q \in \mathbb{R}^{d_{\text{model}} \times d_k})
        
    - (W_K \in \mathbb{R}^{d_{\text{model}} \times d_k})
        
    - (W_V \in \mathbb{R}^{d_{\text{model}} \times d_v})
        

Then for each token (t):

[  
Q_t = x_t W_Q, \quad  
K_t = x_t W_K, \quad  
V_t = x_t W_V  
]

For all tokens (1..t), stack them:

- (Q_{1:t} \in \mathbb{R}^{t \times d_k})
    
- (K_{1:t} \in \mathbb{R}^{t \times d_k})
    
- (V_{1:t} \in \mathbb{R}^{t \times d_v})
    

### 3.2. Attention for token t

In causal self-attention, token (t) attends only to tokens (1..t). For a single head:

[  
\text{Attention}(Q_t, K_{1:t}, V_{1:t}) = \text{softmax}\left( \frac{Q_t K_{1:t}^\top}{\sqrt{d_k}} + M \right)V_{1:t}  
]

- (Q_t) is shape ((1, d_k))
    
- (K_{1:t}^\top) is ((d_k, t))
    
- So (Q_t K_{1:t}^\top) is ((1, t)): scores for each past token.
    
- (M) is the **mask** (e.g., (-\infty) for disallowed positions; for causal it hides _future_ tokens, but not past ones).
    
- After softmax, we get attention weights (\alpha_{t,1}, \dots, \alpha_{t,t}).
    
- Multiply with (V_{1:t}) to get the output vector for token (t).
    

So the computational hotspots:

1. Computing all **Q/K/V**: matrix multiplies with (W_Q, W_K, W_V).
    
2. Doing **QKᵀ** which is (O(t^2 d_k)) over the whole sequence.
    

The KV cache specifically targets **(1)** for _past_ tokens: we don’t want to recompute their K and V again and again.

---

### ❓ Questions (math-level)

7. If we have (t) tokens and head dimension (d_k), what is the shape of the matrix (Q_{1:t} K_{1:t}^\top)?
    
8. Why do we divide by (\sqrt{d_k}) in the attention formula? What happens if (d_k) gets large but we don’t scale?
    
9. First-principles:
    
    > _What information do keys and queries represent conceptually?_  
    > (Hint: think of queries as “questions” and keys as “indexes/tags” that tell which positions are relevant.)
    

---

## 4. What exactly is cached?

Now we connect the math to the **cache**.

### 4.1. Without cache (naive decoding)

At step (t), to get the attention output at each layer you:

1. Recompute (Q_{1:t} = X_{1:t} W_Q)
    
2. Recompute (K_{1:t} = X_{1:t} W_K)
    
3. Recompute (V_{1:t} = X_{1:t} W_V)
    

So you recalc (K_1, \dots, K_{t-1}) and (V_1, \dots, V_{t-1}) every step.

### 4.2. With KV cache (efficient decoding)

Instead, we store **for each layer and head**:

- A **K cache**:  
    [  
    K^{(\ell, h)}_{\text{cache}} \in \mathbb{R}^{T_{\text{max}} \times d_k}  
    ]
    
- A **V cache**:  
    [  
    V^{(\ell, h)}_{\text{cache}} \in \mathbb{R}^{T_{\text{max}} \times d_v}  
    ]
    

At time step (t):

1. We already have cached (K_1, \dots, K_{t-1}) and (V_1, \dots, V_{t-1}).
    
2. We compute the new hidden state (x_t) at each layer (using attention and MLP).
    
3. From (x_t), we compute:  
    [  
    Q_t = x_t W_Q,\quad K_t = x_t W_K,\quad V_t = x_t W_V  
    ]
    
4. We **append**:
    
    - (K_t) to the K cache (now length (t))
        
    - (V_t) to the V cache
        
5. When computing attention at step (t), we use:
    
    - queries: (Q_t)
        
    - keys: cached (K_1..K_t)
        
    - values: cached (V_1..V_t)
        

So the cache is literally just:

> “A tensor with all previous keys and values for each layer/head, grown one token at a time.”

---

### 4.3. Complexity comparison (rough)

Assume:

- Sequence length (T)
    
- Hidden size dimension (d)
    
- Ignoring constants, and focusing on cost per step.
    

**Without cache**:

- At step (t): compute K/V/Q for _all_ (t) tokens → (O(t d))
    
- Total over steps 1..T → (O(T^2 d))
    

**With cache**:

- At step (t): compute K/V/Q only for the new token → (O(d))
    
- Total over steps 1..T → (O(T d))
    

So we trade:

- **Time**: (O(T^2)) → (O(T))
    
- **Memory**: store all (K) and (V): (O(T d)) per layer/head.
    

(There’s still an (O(T^2)) component for QKᵀ if you re-attend to all past tokens at every step, but modern implementations optimize that heavily; for your level, the core “avoid repeated KV computation” is the focus.)

---

### ❓ Questions (what is cached & why)

10. Imagine you are implementing a single attention layer. Precisely **which tensors** will you store in the KV cache, and what shapes do they have if:
    
    - batch size = (B)
        
    - max sequence length = (T)
        
    - number of heads = (H)
        
    - head dimension = (d_k)
        
11. From a memory perspective:
    
    > _Why do we typically cache K and V, but **not** Q?_
    
12. Suppose your sequence gets very long (say 8k or 32k tokens).
    
    - What happens to the **size** of your KV cache?
        
    - What kind of engineering tricks might you consider to keep memory under control?
        

---

## 5. Tradeoffs, limitations, and practical considerations

### 5.1. Where KV cache is used

- **Inference / decoding**: KV cache is crucial for speed.
    
- **Training** or **full-sequence evaluation** where you pass whole sequence at once: you often don’t need a cache in the same way, because you do everything in one big batched computation.
    

### 5.2. Memory vs speed

KV cache:

- Speeds up decoding massively (especially for long contexts and many layers).
    
- But uses memory proportional to:  
    [  
    O(\text{batch size} \times \text{num layers} \times \text{num heads} \times T \times d_k)  
    ]
    

This is why:

- Long-context models can be memory-hungry at inference.
    
- Serving many users in parallel requires careful KV memory management.
    

### 5.3. Truncation / streaming

For **very long streams** (e.g., chat history, streaming input):

- You might **evict** old keys/values (e.g., keep only last N tokens in cache).
    
- Or compress/merge them in some way (e.g., pooling, attention sinks, etc.).
    
- That’s basically saying: “I’ll forget some detailed past in order to save memory.”
    

---

### ❓ Questions (design & tradeoffs)

13. You’re designing a real-time LLM service with strict latency SLAs.
    
    - Why is KV caching almost mandatory in this setting?
        
14. Suppose you must serve 100 users simultaneously with long contexts.
    
    - What are two strategies you might use to reduce KV cache memory usage, even if it hurts performance or context slightly?
        
15. Explain in your own words:
    
    > _How does KV caching change the **time complexity** and **memory complexity** of autoregressive decoding?_
    

---

## 6. Quick recap (you should be able to re-explain this)

Here’s the whole thing in one tight paragraph you should be able to reproduce:

> In a transformer decoder, each new token needs to attend to all previous tokens using self-attention. This requires keys and values for all past tokens. Naively, at decoding step (t), we recompute keys and values for all (t) tokens, leading to roughly (O(T^2)) work over a sequence of length (T). However, the representations of past tokens don’t change in pure autoregressive decoding, so their keys and values are constant. A KV cache simply stores these keys and values for each layer and head when they’re first computed, and then reuses them at future steps, computing only the new query, key, and value for the latest token. This trades extra memory, (O(T d)), for much lower time cost, roughly (O(T d)) instead of (O(T^2 d)), making long-context, low-latency generation feasible.

---

If you’d like, next step we can:

- Go through a **tiny numerical example** (e.g., 3 tokens, (d_k=2)),
    
- Or sketch how you’d **implement KV cache** in code (PyTorch-style shapes and updates),
    
- Or you can answer some of the questions above and I’ll give feedback / push deeper.

---

# ✅ Q14 — _How do you reduce KV cache memory when serving many users?_

First principles:  
The KV cache stores **(num_layers × num_heads × sequence_length × head_dim × 2)** floats per user (for K _and_ V).

When you serve **100 users**, you multiply that cost by 100.  
So KV cache memory explodes.

To reduce memory, you must reduce **either**:

1. The amount of _past context_ you store
    
2. The _dimensionality_ or _precision_ of what you store
    
3. The number of entries (tokens) per user
    
4. The number of layers/heads that store KV
    
5. Or you store it **compressed**
    

Here are the most common strategies:

---

## **Strategy 1 — Limit or truncate the stored context window**

This is the simplest.

Instead of storing all tokens, you only keep the **most recent N tokens**.

Example:

- Model supports 8k tokens
    
- You keep only the last 1k tokens in cache
    
- Older tokens are “forgotten” (evicted)
    

This reduces KV memory **proportionally**.

**Tradeoff:**  
Model has less context → reduced reasoning quality on long sequences.

---

## **Strategy 2 — Use lower precision for KV tensors (quantization)**

KV tensors are typically stored in FP16 or BF16.  
You can quantize them further:

- **FP8**
    
- **INT8**
    
- **INT4** (research/experimental)
    

This cuts memory by **2× to 4×** with small quality loss.

**Tradeoff:**  
Lower precision → Slight accuracy degradation  
But still very effective.

---

## **Strategy 3 — Use fewer layers for caching (selective KV caching)**

This is less known but extremely effective.

Observation:

- Not all layers contribute equally to long-range attention.
    
- Early layers sometimes mostly capture local structure.
    

Technique:

- Cache keys/values only for a **subset** of layers (e.g., top half).
    
- For the rest, recompute on the fly.
    

**Tradeoff:**  
Saves memory  
But increases compute and may affect quality slightly.

---

## **Strategy 4 — Compress KV tensors (e.g., pooling or low-rank approximations)**

Some research methods:

- **Group tokens** and store only one representative key/value per group
    
- **Low-rank projection** of K and V (reduce dimension from d_k → r)
    
- **Asymmetric quantization**
    

These reduce the KV memory footprint _without_ fully throwing away old tokens.

**Tradeoff:**  
Some quality loss, but often acceptable.

---

## **Strategy 5 — Cache KV on CPU or NVMe (tiered KV caching)**

This is for extremely long sequences or many users.

- Keep the most recent N tokens in fast GPU memory
    
- Move older K/V to CPU (slower but bigger)
    
- Move ancient K/V to disk (NVMe)
    

Only a small sliding window stays on GPU.

**Tradeoff:**  
Latency increases when model occasionally fetches from slower memory.

---

### Summary Table

|Strategy|Memory Saving|Tradeoff|
|---|---|---|
|Truncate context|High|Lose long context|
|KV quantization|Moderate–High|Slight quality hit|
|Selective KV caching|Moderate|Recompute some layers|
|KV compression|Moderate–High|Approximation errors|
|Tiered cache (CPU/NVMe)|Very High|Latency spikes|

---

# 🔍 Now your turn — reasoning questions

Answer these to internalize the ideas:

### **Q14.1**

Why does truncating KV cache directly reduce memory linearly?

### **Q14.2**

Suppose you quantize KV from FP16 → INT8.  
By what factor do you reduce KV memory?  
(Hint: FP16 = 2 bytes, INT8 = 1 byte)

### **Q14.3**

Why might caching only top-half layers work better than caching only bottom-half layers?

### **Q14.4**

If you're serving 100 users and each user uses a 4k context,  
why might KV quantization alone not be enough?

---

If you answer these, I can give feedback and we can move deeper into:

- KV eviction policies
    
- FlashAttention & KV
    
- Prefix caching
    
- Multi-user batching with shared KV cache
    
- Key-value recomputation vs prefill
    

Just let me know how deep you want to go.

---

# ✅ **14.1 — “Because you are using fewer tokens”**

Correct. But let's state it fully:

If KV cache stores **one K and one V vector per token**, then memory is:

[  
\text{Memory} = O(T \cdot d_k \cdot n_{\text{layers}} \cdot n_{\text{heads}})  
]

So if you cut stored tokens from (T) to (T'):

[  
\text{Memory ratio} = \frac{T'}{T}  
]

**Linear** reduction.

👉 Your intuition is right.  
👉 Stating it in formula form shows deeper understanding.

---

# ✅ **14.2 — Factor of 2**

Perfect.

- FP16 = 16 bits = 2 bytes
    
- INT8 = 8 bits = 1 byte
    

So memory becomes:

[  
\text{Memory}_{\text{INT8}} = \frac{1}{2} \text{Memory}_{\text{FP16}}  
]

---

# ⚠️ **14.3 — Your answer is basically correct but not complete**

You said:

> capture more high level features which will be more influential…

Yes.

Let’s articulate it clearly:

### Why top layers matter more for KV caching:

- Early layers capture **local** patterns (syntax, token-level roles)
    
- Higher layers capture **global reasoning structure**, long-distance relations, and semantic meaning
    
- Attention from higher layers contributes more strongly to final logits
    

If you drop early-layer KVs:

- Model can reconstruct many short-range patterns on the fly
    
- But you preserve the long-range dependencies that matter more
    

Therefore:

👉 **Caching top layers preserves global knowledge**  
👉 **Caching bottom layers is less helpful**

---

# ⚠️ **14.4 — You got the direction right but reasoning needs to be sharper**

You said:

> yes still processing 400000 which still requires a lot of KV cash … if you can truncated the input then we can save a lot , or use tiered kv cache.

Let’s reason from first principles:

Suppose:

- 100 users
    
- Each with 4k tokens cached
    
- 40 layers
    
- 32 heads
    
- head_dim = 128
    
- FP16
    

Then per user KV memory is approximately:

[  
\sim 40 \times 32 \times 4000 \times 128 \times 2 \text{ bytes} \approx 1.3 \text{ GB}  
]

For **100 users**:

[  
\approx 130 \text{ GB}  
]

Even if you quantize KV to INT8 (×0.5), you still need:

[  
65 \text{ GB}  
]

A single GPU cannot store that.

This is the correct reasoning:

**KV quantization reduces memory, but not enough when:**

- You have many concurrent users, AND
    
- Sequence lengths are large, AND
    
- Models have many layers/heads
    

Thus you need:

- truncation
    
- tiered caching
    
- sharing / reusing prefix KVs
    
- multi-query attention (reduces heads)
    
- low-rank KV
    
- or mixture-of-experts that reduces active layers
    

So your answer was directionally right; now it matches real operational constraints.

---

# 🔍 **Follow-up reasoning questions (deeper thinking)**

Try these — they force you to connect ML architecture with systems design.

### **Q14.5**

Why does multi-query attention (MQA) massively reduce KV cache memory, even if the model still has many attention heads?

(Hint: MQA shares K and V across heads.)

---

### **Q14.6**

Imagine you build a service that must keep KV cache for each user.  
How would _prefix caching_ help reduce memory when multiple users share the same long prompt (e.g., system prompt + few-shot examples)?

---

### **Q14.7**

If you truncate KV cache to last N tokens, which types of tasks degrade the most, and why?

(Hint: tasks requiring retrieval of far-past information.)

---

### **Q14.8**

Why does tiered KV caching (GPU → CPU → NVMe) generally work well in _streaming_ inference but poorly in _batch long-context inference_?

---

Answer these, and you’ll have internalized KV cache at the level of someone who can design LLM inference systems.