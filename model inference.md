---
tags:
  - gpu
---
- [Inside vLLM: Anatomy of a High-Throughput LLM Inference System - Aleksa Gordić](https://www.aleksagordic.com/blog/vllm)  
- [GitHub - GeeeekExplorer/nano-vllm: Nano vLLM](https://github.com/GeeeekExplorer/nano-vllm)  
- [Life of an inference request (vLLM V1): How LLMs are served efficiently at scale](https://www.ubicloud.com/blog/life-of-an-inference-request-vllm-v1)


### The KV Cache Bottleneck

In a transformer-based LLM, the core of the self-attention mechanism is the computation of **Query (Q), Key (K), and Value (V)** vectors for each token in the input sequence. For an autoregressive model to generate the next token, it needs to attend to all the previous tokens. Without any optimizations, this would mean recomputing the Q, K, and V vectors for the _entire_ sequence every time a new token is generated. This leads to a quadratic computational complexity, O(N2), where N is the sequence length. This is computationally prohibitive for long sequences.

The **KV cache** is a simple but powerful optimization to solve this. As the model processes the input prompt, it calculates the K and V tensors for each token. It then caches these tensors in GPU memory. When the model needs to generate the next token, it only needs to compute the Q, K, and V for that single new token and append them to the cache. The new Q vector is then used to compute attention scores against the full set of cached K and V vectors. This reduces the computational complexity of the decoding step from O(N2) to O(N), which is a massive speedup.

However, the KV cache itself becomes the primary bottleneck for two key reasons:

1. **Linear Memory Growth:** The size of the KV cache grows **linearly** with the sequence length and the batch size. For a single sequence of length N, the total VRAM required for the KV cache across all layers is roughly 2×N×L×H×Dh​, where L is the number of layers, H is the number of heads, and Dh​ is the head dimension. For a large model with a long context window (e.g., 128k tokens), the KV cache can consume tens or even hundreds of gigabytes of VRAM. Since GPU memory is a finite and expensive resource, this becomes the limiting factor for how many users you can serve concurrently or how long a conversation can be.
    
2. **Memory Fragmentation:** Traditional inference engines allocate a contiguous block of VRAM for each request's KV cache. However, different requests have different sequence lengths. When a request completes, its memory is freed, but it may leave a gap that is too small for a new incoming request. This leads to severe **external memory fragmentation**, which results in wasted VRAM and low GPU utilization. The GPU might have enough total VRAM for a new request, but no single contiguous block is large enough, forcing it to wait or reject the request.
    

You can find more detailed technical explanations in these excellent resources:

- **vLLM's PagedAttention Paper:** The original paper introduces PagedAttention, a revolutionary technique that solves the memory fragmentation problem by treating the KV cache like an operating system's virtual memory, breaking it into fixed-size blocks. (Search for the paper from the vLLM team at UC Berkeley).
    
- **Hugging Face Blog on KV Caching:** A great introductory blog post that walks through the mechanics of KV caching.
    
- **BentoML's LLM Inference Handbook:** A comprehensive guide that covers KV cache offloading and other optimization techniques.
- 
