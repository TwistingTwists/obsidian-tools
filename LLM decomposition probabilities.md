# Task decomposition probabilities

[Summary from kimi](https://www.notion.so/Summary-from-kimi-2adf62ce42418025a0a7d6f50d300993?pvs=21)

---

# Understanding How Task Decomposition Affects Voting-Based Reliability: A Detailed Summary

## The Problem Setup

Consider a complex task that requires **s total steps** to complete successfully. We want to understand how breaking this task into subtasks of different sizes affects the overall success rate when using a voting mechanism to verify correctness.

### Key Parameters

- **s**: Total number of steps in the complete task
- **p**: Per-step success rate (the inherent probability of getting any single step correct)
- **m**: Steps per subtask (the decomposition granularity)
- **k**: Vote margin required to declare a subtask correct
- **Number of subtasks**: $n = s/m$

### The Voting Mechanism

For each subtask, multiple voters attempt to solve it. The correct solution must achieve a margin of **k** votes over any alternative to be accepted. The task succeeds only if *all* subtasks are correctly decided by voting.

### Critical Assumption

The model assumes that for each subtask, the correct solution "races" against only a **single most-likely alternative** incorrect solution. This is a simplifying assumption that becomes increasingly unrealistic (and favorable to large $m$) as subtasks grow larger.

---

## The Core Tradeoff

There is a fundamental tension between two decomposition strategies:

**Fine-grained decomposition (small $m$):**

- Many subtasks ($s/m$ is large)
- Each subtask is easy (high $p_{vote} = p^m$)
- Many opportunities to fail (must succeed on all $s/m$ subtasks)

**Coarse-grained decomposition (large $m$):**

- Few subtasks ($s/m$ is small)
- Each subtask is hard (low $p_{vote} = p^m$)
- Few critical decision points
- **Benefit from favorable assumption** (more on this below)

---

## The Mathematical Framework

The model provides four key equations:

### Equation 10: Probability of Correct Vote

The probability a single voter gets a subtask completely correct. Since the subtask has $m$ independent steps, each with success probability $p$, the probability of getting all $m$ steps right is $p$ multiplied by itself $m$ times.

$$p_{vote} = p^m$$

> Example: If $p = 0.9$ and $m = 3$, then $p_{vote} = 0.9^3 = 0.729$
> 

### Equation 11: Probability of Alternative Error

The probability a voter produces the specific "most likely alternative" wrong answer. This represents getting exactly ($m-1$) steps correct and exactly 1 step wrong.

$$p_{alt} = (1-p)p^{m-1}$$

> Example: If $p = 0.9$ and $m = 3$, then $p_{alt} = 0.1 \times 0.9^2 = 0.081$
> 

### Equation 12: Subtask Success Probability

The probability that voting succeeds for one subtask. This comes from gambler's ruin theory: imagine a random walk where each vote pushes toward either "correct" or "alternative," and correct must get $k$ steps ahead to win.

$$p_{sub} = \frac{p_{vote}^k}{p_{vote}^k + p_{alt}^k}$$

**Simplified form:**
$$p_{sub} = \frac{1}{1 + (\frac{1-p}{p})^k}$$

This shows that what matters is not the absolute probabilities but their **ratio**.

### Equation 13: Full Task Success Probability

The probability of completing the entire task successfully. Since there are $s/m$ subtasks and all must succeed independently:

$$p_{full} = p_{sub}^{(s/m)}$$

$$p_{full} = \left(1 + \left(\frac{1-p}{p}\right)^k\right)^{-s/m}$$

---

## The "Vanishing Fraction" Phenomenon

Here is where the model's assumption becomes crucial and increasingly problematic for large $m$.

### What Happens as $m$ Grows

When a subtask involves $m$ steps, there are **many ways to be wrong**:

- **For $m = 1$:**
    - 1 correct solution
    - 1 wrong alternative
    - *The wrong alternative captures 100% of error probability*
- **For $m = 2$:**
    - 1 correct solution (both steps right)
    - 3 wrong sequences (wrong on step 1 only, wrong on step 2 only, wrong on both)
    - *Most likely wrong answer captures ~2/3 of error probability*
- **For $m = 10$:**
    - 1 correct solution
    - 1,023 wrong sequences
    - *The most likely wrong alternatives (10 sequences with exactly 1 error) capture ~60% of error probability collectively*
    - *Any single alternative captures a small fraction*
- **For $m = 50$:**
    - 1 correct solution
    - Over $10^{15}$ wrong sequences
    - *Error probability is spread extremely thin across exponentially many alternatives*
    - *Any single alternative captures a vanishing fraction of total error mass*

### Why This Matters

The model assumes that voters choose between:

1. The correct answer (probability $p^m$)
2. One specific alternative (probability $(1-p)p^{m-1}$)

But in reality with large $m$:

- Voters who make errors would produce **many different wrong answers**.
- Votes would be **highly fragmented** among countless alternatives.
- No single alternative would have probability as high as $(1-p)p^{m-1}$.
- The correct answer would have an easier time winning by margin $k$.

The model's assumption **concentrates error mass** into one alternative, making voting seem harder than it really is for large $m$. This makes the model **biased in favor of coarse decomposition**.

---

## The Surprising Constant Ratio

Despite both probabilities vanishing as $m$ grows, something remarkable happens:

**The ratio $p_{alt} / p_{vote}$ remains constant:**

$$\frac{p_{alt}}{p_{vote}} = \frac{(1-p)p^{m-1}}{p^m} = \frac{1-p}{p}$$

This ratio is **independent of m**!

### What This Means

Even though:

- $p_{vote} = p^m \rightarrow 0$ (correct answers become rare)
- $p_{alt} = (1-p)p^{m-1} \rightarrow 0$ (alternatives become rare)

They decrease at the **same relative rate**, so the voting difficulty per subtask (measured by this ratio) stays constant regardless of decomposition level.

### Why the Assumption Helps Large $m$

- **The Model Says:** "The difficulty ratio is always $(1-p)/p$, regardless of $m$."
- **Reality Would Say:** "With large $m$, errors fragment across exponentially many alternatives, so the effective ratio competing against correct should be much smaller, making voting easier for large $m$."

By keeping the ratio constant, the model **doesn't give large $m$ the full credit** it would get from error fragmentation. Yet even with this conservative treatment, the analysis can show whether fine-grained decomposition wins.

---

## Implications for Analysis

This framework reveals that the model is set up with assumptions that **favor coarse decomposition** (large $m$):

1. **Fewer decision points:** Only $s/m$ subtasks need to succeed.
2. **Concentrated alternatives:** Error probability artificially concentrated into one competitor.
3. **Constant difficulty ratio:** Large $m$ doesn't face the reality of fragmented opposition.

**Therefore:**

- If the analysis shows **small $m$ wins** $\rightarrow$ Decomposition genuinely helps, even with conservative assumptions.
- If the analysis shows **large $m$ wins** $\rightarrow$ Inconclusive; might only win due to favorable assumptions.
- The vanishing fraction insight helps us understand the **bias built into the model**.

---

## Key Takeaway

The "vanishing fraction" phenomenon reveals that as you make subtasks larger (increase $m$), the model's assumption that "the correct solution races against a single most-likely alternative" becomes increasingly unrealistic and generous to coarse decomposition. In reality, with large $m$, the error probability would be spread so thinly across exponentially many alternatives that the correct answer would have a much easier time winning votes.

The model maintains a constant difficulty ratio $(1-p)/p$ regardless of $m$, which doesn't reflect this fragmentation benefit that large $m$ would actually enjoy. This makes the framework a **conservative test**: if fine-grained decomposition still wins despite these favorable assumptions for coarse decomposition, that's strong evidence for the benefits of breaking complex tasks into smaller, more manageable pieces.