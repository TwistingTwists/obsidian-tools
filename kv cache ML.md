**Detailed Prompt Rewrite:**

Create a set of flashcards that teach first principles thinking through the lens of ML system design interviews, specifically focusing on **KV cache** (key-value cache) mechanisms in transformer architectures. Each flashcard should:

- **Start from fundamentals**: Break down KV cache into its core components, underlying assumptions, and mathematical/physical constraints
- **Use Socratic questioning**: Pose questions that force derivation from scratch rather than recall of facts
- **Connect to interview scenarios**: Frame questions around real ML system tradeoffs (latency, memory, throughput, cost)
- **Include numerical reasoning**: Ask for back-of-envelope calculations that reveal scaling laws and bottlenecks
- **Provoke edge case analysis**: Challenge assumptions by asking "what if" questions that break standard implementations
- **Cover the full stack**: Move from mathematical foundations to GPU kernels to distributed systems implications

The flashcards should be structured to make the interviewee *derive* why KV cache exists, its design choices, and its limitations, rather than just describing what it is.

---

**Flashcard Set: First Principles of KV Cache**

---

**Card 1: The Computational Redundancy Problem**
Q: In autoregressive transformer decoding, why must we recompute the exact same key/value tensors for previously-generated tokens at each new step? Derive the FLOP waste for a 70B parameter model generating a 4096-token sequence.

Q: If attention is QK^T, and Q changes every step but K for token i is constant after its generation, what does this imply about the computational graph's redundancy? Calculate the percentage of redundant matrix multiplications at step 2000.

---

**Card 2: Memory vs Compute Tradeoff Fundamentals**
Q: From first principles: Is KV cache a memory optimization or a compute optimization? Prove your answer by writing the cost function: `total_cost = memory_bandwidth_cost + compute_cost` for both cached and non-cached scenarios.

Q: A100 has 1.5TB/s memory bandwidth and 312 TFLOPS. Given K/V tensors of shape `[batch, seq_len, n_heads, head_dim]`, derive the breakeven sequence length where caching becomes slower than recomputation due to memory pressure.

---

**Card 3: The Slot Reservation Paradox**
Q: Why can't we allocate KV cache memory dynamically as sequence length grows? Consider: GPU memory allocation latency, tensor contiguousness requirements for GEMMs, and the ring attention communication pattern. What's the fundamental hardware constraint?

Q: If we pre-allocate for max_seq_len=8192 but 90% of requests are shorter than 512 tokens, derive the memory fragmentation ratio. How does this challenge the "memory is cheap" assumption?

---

**Card 4: Precision & Quantization Impact**
Q: KV cache is often FP16 during training. For a 1T parameter model serving, calculate the memory footprint difference between FP16 vs INT8 cache for batch=256, seq_len=2048. Now derive the error propagation: how does quantization in K/V affect the attention softmax numerically?

Q: If we quantize K-cache per-token but V-cache per-channel, what does this reveal about which component is more sensitive to precision? Prove using the entropy gradient w.r.t K vs V.

---

**Card 5: The Batch Size Collapse Problem**
Q: In continuous batching, new requests arrive while others are mid-generation. Why must KV cache slots be pre-allocated with *request-level isolation*? What happens to memory efficiency when 32 requests have wildly different sequence lengths?

Q: Derive the effective batch size as a function of memory: `B_eff = memory_available / (2 * L * d_model * bytes_per_param)`. If L doubles, what happens to throughput?

---

**Card 6: Attention Mask & Position Embedding Coupling**
Q: Why does rotating KV cache (for RoPE) require fundamentally different memory access patterns than static position embeddings? Draw the memory layout and derive the extra FLOPs for RoPE application per step.

Q: If we use ALiBi instead of RoPE, does the KV cache shape change? What does this imply about the coupling between positional encoding and cache invalidation strategies?

---

**Card 7: Distributed Cache Consistency**
Q: In tensor parallelism across 8 GPUs, each holding a shard of K/V heads, what happens to cache coherence when a request is preempted and resumed on a different set of GPUs? Derive the AllGather latency for cache restoration.

Q: For pipeline parallelism, why must KV cache for the *same request* be stored across all stages? Calculate the data duplication factor and explain why this breaks the "cache avoids recomputation" principle at pipeline boundaries.

---

**Card 8: The Eviction Strategy Dilemma**
Q: If we implement LRU eviction for KV cache under memory pressure, why does this necessarily break attention's causal mask? Prove mathematically that dropping arbitrary K/V pairs introduces information leakage from future tokens.

Q: Design a principled eviction policy from scratch: Should you evict by attention score magnitude, token frequency, or temporal locality? Derive the optimal policy by minimizing the expected attention reconstruction error.

---

**Card 9: FlashAttention & IO-Awareness**
Q: FlashAttention recomputes attention during backward pass to save memory. Why is this approach *inverted* for KV cache in inference? Compare the SRAM/HBM access patterns for both scenarios and identify the hardware hierarchy assumption that flips the optimal strategy.

Q: Calculate the arithmetic intensity of KV cache read vs Q-projection compute. At what head dimension does the operation become memory-bound vs compute-bound?

---

**Card 10: The Fundamental Assumption Breaker**
Q: What if we replace softmax attention with linear attention? Re-derive the KV cache necessity from scratch. Does it still exist? If not, what new caching structure emerges and why?

Q: For state-space models (Mamba), there's no KV cache. Explain from first principles: What structural difference in the recurrence makes this possible, and what capability do we lose in exchange?

---

**Card 11: Power & Cost Modeling**
Q: A KV lookup consumes 0.5 pJ/bit while a FP16 FMA consumes 5 pJ/op. For generating one token on Llama-70B, calculate the energy ratio: memory_access_energy / compute_energy. What does this reveal about the true optimization target?

Q: If H100 memory costs $X/GB/month and compute costs $Y/TFLOP-hour, derive the cost-optimal sequence length to maximize caching.

---

**Card 12: The Interview Trap**
Q: Your candidate says "KV cache speeds up generation by avoiding recomputation." Write 3 follow-up questions that expose whether they derived this or memorized it, each drilling into a different failure mode (numerical, architectural, or distributed).

Q: Design a scenario where KV cache *slows down* total system throughput. Defend your answer with queueing theory.