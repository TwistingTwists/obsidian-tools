
Shannon entropy is a key concept from **information theory** that measures the **average level of uncertainty or "surprise"** inherent in a random variable's possible outcomes.1

In simpler terms, it quantifies how unpredictable a piece of information is.2

- **Low Entropy:** A predictable source has **low entropy**.3
    
    - _Example:_ A loaded coin that lands on "heads" 99% of the time. You are not very "surprised" when you see heads, so the information content is low. An event that is _certain_ (like the sun rising in the east) has zero entropy.4
        
- **High Entropy:** An unpredictable source has **high entropy**.5
    
    - _Example:_ A fair coin flip (50% heads, 50% tails). The outcome is as uncertain as it can be. Learning the result provides the maximum amount of information for a two-outcome system.
        

---

### 📈 The Formula

Shannon entropy, denoted by $H$, is calculated as the sum of the probabilities of each outcome multiplied by their "surprisal."

For a random variable $X$ with $n$ possible outcomes $\{x_1, ..., x_n\}$, the formula is:

$$H(X) = - \sum_{i=1}^{n} p(x_i) \log_2(p(x_i))$$

Let's break that down:

- $p(x_i)$: This is the **probability** of the 6$i$-th outcome occurring.7
    
- $\log_2(p(x_i))$: This part is called the **"surprisal"** of the outcome.
    
    - If an event is very likely (e.g., 8$p = 0.99$), its surprisal is very low.9
        
    - If an event is very rare (e.g., $p = 0.01$), its surprisal is very high.
        
- $\log_2$: The logarithm is **base-2** because the unit of Shannon entropy is the **"bit."10**
    
- $- \sum$: The **negative sum** sign just ensures the final value is positive (since the log of a probability $\leq 1$ is always negative or zero).
    

---

### 🪙 Example: The Coin Flip

Let's compare the entropy of a biased coin vs. a fair coin.

**Case 1: The Biased Coin (90% Heads, 10% Tails)**

- $p(\text{Heads}) = 0.9$
    
- $p(\text{Tails}) = 0.1$
    
- $H = - [ (0.9 \cdot \log_2(0.9)) + (0.1 \cdot \log_2(0.1)) ]$
    
- $H \approx - [ (0.9 \cdot -0.152) + (0.1 \cdot -3.322) ]$
    
- $H \approx - [ -0.137 - 0.332 ] \approx \mathbf{0.469 \text{ bits}}$
    
- The uncertainty is low (less than half a bit).
    

**Case 2: The Fair Coin (50% Heads, 50% Tails)**

- $p(\text{Heads}) = 0.5$
    
- $p(\text{Tails}) = 0.5$
    
- $H = - [ (0.5 \cdot \log_2(0.5)) + (0.5 \cdot \log_2(0.5)) ]$
    
- $H = - [ (0.5 \cdot -1) + (0.5 \cdot -1) ]$
    
- $H = - [ -0.5 - 0.5 ] = \mathbf{1 \text{ bit}}$
    
- This is the **maximum possible entropy** for a two-outcome system. It means we need exactly one bit, on average, to communicate the result of the flip.
    

---

### Why Is It Important?

Shannon entropy is a foundational idea with many applications:

- **Data Compression:** It sets the **absolute theoretical limit** for data compression.11 A file's entropy (in bits) is the minimum average number of bits per symbol needed to encode it. This is why a random file (high entropy) is almost impossible to compress, while a repetitive text file (low entropy) compresses very well.
    
- **Machine Learning:** Used in decision trees (as "Information Gain") to decide which questions are best to "ask" to split the data.12
    
- **Cryptography:** To measure the randomness and unpredictability of cryptographic keys.
    
- **Biology:** To analyze the information content in DNA or protein sequences.
    


----




but help me build intution on how to think about shannon entropy.

1. what is difference between 0.5 vs 0.4 vs 0.3 == sensitivity

2. how to think about 'desriable' values of shannon entroppy in difference scenaris

3. when should I use shannon entropy -- thinking form first principles.

4. why does it even work? shannon entropy is just the sum of probabilities. why can't we take geometric / harmonic mean or something else?