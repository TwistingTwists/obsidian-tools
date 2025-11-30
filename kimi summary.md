# Summary from kimi

---

# Task Decomposition & Voting Reliability: A Three-Phase Explanation

## Phase 1: The TL;DR

- **Core Question:** Does breaking a complex task into smaller subtasks improve success rates under voting?
- **Central Tension:**
    - **Fine-grained (many tiny subtasks):** Each is easy, but one failure kills the whole task.
    - **Coarse-grained (few big subtasks):** Fewer failure points, but each vote is harder.
- **Key Insight:** The mathematical model artificially favors coarse decomposition by assuming all errors concentrate into a *single* wrong answer. In reality, big subtasks produce exponentially many wrong answers that split the error vote.
- **Bottom Line:** If the math shows fine-grained decomposition wins, that's strong evidence it truly works. If coarse wins, the result is suspect—the model may be giving it unfair help.

---

## Phase 2: The Results (Why It Matters)

### The Tradeoff in Plain Terms

When you decompose a task of $s$ steps into subtasks of size $m$:

**Small $m$ (e.g., 1 step per subtask):**

- You need $s$ subtasks all to succeed.
- Each vote is a slam dunk (high $p_{vote} = p$).
- But with many chances to fail, overall reliability tanks.

**Large $m$ (e.g., 50 steps per subtask):**

- Only $s/50$ subtasks to get right.
- Each vote is a long shot (low $p_{vote} = p^{50}$).
- *But the model secretly helps you by pretending there's just one wrong answer.*

### The "Vanishing Fraction" Problem

This is the model's Achilles' heel. For a subtask with $m$ steps:

- **Correct answer:** Exactly 1 sequence (all steps right).
- **Wrong answers:** $2^m - 1$ sequences (any error pattern).

As $m$ grows:

- **At $m=1$:** Wrong answers = 1 $\rightarrow$ 100% of errors go to one alternative.
- **At $m=10$:** Wrong answers = 1,023 $\rightarrow$ Most likely alternatives (exactly 1 error) share 60% of error mass.
- **At $m=50$:** Wrong answers > $10^{15}$ $\rightarrow$ Error probability diffuses across quadrillions of alternatives.

**The model's cheat:** It pretends all this error mass funnels into one competitor, making the correct answer's job look harder than it really is.

### The Constant Ratio Paradox

Here's the weird part—both $p_{vote}$ and $p_{alt}$ shrink to zero as $m$ increases:

- $p_{vote} = p^m$ (correct answer gets rarer)
- $p_{alt} = (1-p)p^{m-1}$ (specific wrong answer gets rarer)

But their ratio never changes:

$$
\frac{p_{alt}}{p_{vote}} = \frac{(1-p)p^{m-1}}{p^m} = \frac{1-p}{p}
$$

This means the "difficulty" of voting stays constant in the model, hiding the real-world advantage large $m$ would have from vote splitting among many wrong answers.

---

## Phase 3: The Equations (Full Derivation)

### Setup Parameters

- $s$: Total steps in full task
- $p$: Success rate per step ($0 < p < 1$)
- $m$: Steps per subtask
- $k$: Vote margin needed to declare winner
- $n = s/m$: Number of subtasks

### Equation 10: Probability One Voter Gets a Subtask Right

**Derivation:** A subtask has $m$ independent steps. To be completely correct, a voter must get every step right.

$$
p_{vote} = P(\text{all } m \text{ steps correct}) = p \times p \times \dots \times p = p^m
$$

> Example:
If $p = 0.9$, $m = 4$:
$$p_{vote} = 0.9^4 = 0.6561$$
> 

### Equation 11: Probability of the Specific Wrong Answer

**Derivation:** The model's "most likely alternative" is the wrong answer with exactly one error and $m-1$ correct steps. There are $m$ ways this can happen (error in step 1, step 2, ..., step m), but the model lumps them into one mythical alternative.

$$
p_{alt} = P(m-1 \text{ steps right AND } 1 \text{ specific step wrong}) = p^{m-1}(1-p)
$$

> Example:
If $p = 0.9$, $m = 4$:
$$p_{alt} = 0.9^3 \times 0.1 = 0.0729$$
> 

### Equation 12: Probability Voting Declares the Right Answer Winner

**Derivation (Gambler's Ruin):** Imagine a random walk where:

- Each correct vote = +1 step
- Each alternative vote = -1 step

Correct wins when it gets $k$ steps ahead. The probability correct reaches $+k$ before $-k$ is:

$$
p_{sub} = \frac{p_{vote}^k}{p_{vote}^k + p_{alt}^k}
$$

**Simplification:** Divide numerator and denominator by $p_{vote}^k$:

$$
p_{sub} = \frac{1}{1 + (\frac{p_{alt}}{p_{vote}})^k} = \frac{1}{1 + (\frac{1-p}{p})^k}
$$

**Key Insight:** Only the ratio $(1-p)/p$ matters, not $m$ itself.

> Example:
If $p = 0.9$, $k = 3$:
$$p_{sub} = \frac{1}{1 + (\frac{0.1}{0.9})^3} = \frac{1}{1 + 0.00137} \approx 0.9986$$
Even though $p_{vote}$ dropped to 0.6561, voting still succeeds 99.86% of the time because wrong answers are rare relative to correct ones.
> 

### The Ratio Derivation (Step-by-Step)

Starting with $p_{alt} = (1-p)p^{m-1}$ and $p_{vote} = p^m$, take the ratio:

$$
\begin{aligned}
\frac{p_{alt}}{p_{vote}} &= \frac{(1-p)p^{m-1}}{p^m} \\
&= (1-p) \times \frac{p^{m-1}}{p^m} \\
&= (1-p) \times p^{m-1-m} \\
&= (1-p) \times p^{-1} \\
&= \frac{1-p}{p}
\end{aligned}
$$

**QED:** The $m$ terms cancel completely. This ratio is invariant under decomposition.

### Equation 13: Probability of Full Task Success

**Derivation:** Since subtask votes are independent and all must succeed:

$$
\begin{aligned}
p_{full} &= P(\text{subtask 1 succeeds AND subtask 2 succeeds} \dots) \\
&= p_{sub} \times p_{sub} \times \dots \times p_{sub} \\
&= p_{sub}^{(s/m)}
\end{aligned}
$$

**Expanded Form:**

$$
p_{full} = \left[1 + \left(\frac{1-p}{p}\right)^k\right]^{-s/m}
$$

> Example:
If $s = 100$, $m = 4$, $p = 0.9$, $k = 3$:
$$p_{full} = 0.9986^{(100/4)} = 0.9986^{25} \approx 0.966$$
> 

---

## Putting It All Together: A Worked Example

**Parameters:** $s = 20$, $p = 0.85$, $k = 2$

**Scenario A (Fine):** $m = 1$ (20 subtasks)

- $p_{vote} = 0.85^1 = 0.85$
- $p_{alt} = 0.15 \times 0.85^0 = 0.15$
- $p_{sub} = \frac{1}{1 + (\frac{0.15}{0.85})^2} = 0.97$
- $p_{full} = 0.97^{20} = \mathbf{0.54}$

**Scenario B (Coarse):** $m = 10$ (2 subtasks)

- $p_{vote} = 0.85^{10} \approx 0.20$
- $p_{alt} = 0.15 \times 0.85^9 \approx 0.033$
- $p_{sub} = \frac{1}{1 + (\frac{0.033}{0.20})^2} \approx 0.97$
- $p_{full} = 0.97^2 \approx \mathbf{0.94}$

**The Illusion:** Coarse looks better ($0.94 > 0.54$). But in reality, with $m = 10$, errors would fragment across 1,000 wrong answers, making the effective $p_{alt}$ much smaller and $p_{sub}$ truly higher. The model's 0.97 is **conservative**—reality would be even better for coarse decomposition.

**Conclusion:** Only trust the result when fine-grained still wins despite this hidden penalty.