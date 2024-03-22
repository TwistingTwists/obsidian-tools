---
tags:
  - research_papers
---
 
 [https://github.com/dair-ai/ML-Papers-of-the-Week](https://github.com/dair-ai/ML-Papers-of-the-Week) 

----

## MORA 
[Paper](https://arxiv.org/abs/2403.13248) 


LLAMA Factory 
[Paper](https://huggingface.co/papers/2403.13372?utm_source=digest-papers&utm_medium=email&utm_campaign=2024-03-21)

https://huggingface.co/papers/2403.13043?utm_source=digest-papers&utm_medium=email&utm_campaign=2024-03-21

### Overview of Detection Transformers

TABLE 4: Overview of Advantages and limitations of Detection Transformers.

https://arxiv.org/pdf/2306.04670.pdf
### Gemma - Google 
https://www.kaggle.com/models/google/gemma
https://storage.googleapis.com/deepmind-media/gemma/gemma-report.pdf

----

###  Towards Cross-Tokenizer Distillation: the Universal Logit Distillation Loss for LLMs

---

### Learning to Learn Faster from Human Feedback with Language Model Predictive Control 

2024-02-21 11:30:08

TL;DR
1.  Finetune your LLM on successful chat sessions w/ humans. During inference generate multiple chat futures and execute the first step of the shortest generated future, like a recursive Dijkstra’s over LLM chat.
2. Only need to fine tune once. Second time fine tuning does not provide much benefit
3. 

---

## Self Rewarding LLM 

2024-02-21 11:51:26

 TL;DR
 1. [Good Summary ](https://www.youtube.com/watch?v=PeSLWTZ1Yg8&t=2s)

---

### Unnatural Instructions: Tuning Language Models with (Almost) No Human Labor [Source](https://arxiv.org/pdf/2212.09689.pdf)

TL;DR
1. Unnatural Instructions: a large dataset of creative and diverse instructions, collected with virtually no human labor.
	1. 240,000 examples of instructions, inputs, and outputs.
2. Generates examples via the LLM => measures the *correctness* and *diversity* of generated data
	1. covers variety of tasks - like Question Answering, Sentiment Analysis, Arithmetic, Geometry, Event Ordering, Puzzles, etc
	2. correctness = 56% instructions
	3. incorrect = 5% incomprehensible, 18% output does not match the task given , 20% incorrect output
	4. Diversity = via BERTScore
3. FineTune the T5-LLM model


----

### Reinforced Self-Training (ReST) for Language Modeling [Source](https://arxiv.org/abs/2308.08998)

----

### Open-World Object Manipulation using Pre-trained Vision-Language Models [Source](https://arxiv.org/abs/2303.00905)

---

### RT-2: Vision-Language-Action Models Transfer Web Knowledge to Robotic Control

---

### Q-Transformer: Scalable Offline Reinforcement Learning via Autoregressive Q-Functions [Source](https://arxiv.org/abs/2309.10150)

---

### Scalable Multi-Agent Reinforcement Learning through Intelligent Information Aggregation [Source ](https://arxiv.org/abs/2211.02127)

---


### Alexa Teacher Model: Pretraining and Distilling Multi-Billion-Parameter Encoders for Natural Language Understanding Systems [Source](https://arxiv.org/abs/2206.07808)


----

### Beyond the Imitation Game: Quantifying and extrapolating the capabilities of language models [Source](https://arxiv.org/abs/2206.04615)

---


### "How Robust r u?": Evaluating Task-Oriented Dialogue Systems on Spoken Conversations [Source](https://arxiv.org/abs/2109.13489
)

---

### Beyond Domain APIs: Task-oriented Conversational Modeling with Unstructured Knowledge Access [Source](https://arxiv.org/abs/2006.03533)

---

### LLM Agents 
 
#### What

Agent = Large Language Model (LLM) + MOTA  

M = Memory   =  Context :: Global (e.g. python ecosystem) + local (codebase) 

O = Observation = How context changes? 

T = thinking  = Why

A = Action = Based on observation + thinking => produce a effect.

   e.g Git Diff - easier for programmer to act

 

#### Failing
When you watch the transcripts of LLM agents failing to do a task, it’s often one of three things:

1. The agent starts going in circles, visiting the same goals.

2. The agent gets “lazy”, takes a high-level goal and splits it into a subgoal that’s just “actually do the work”, and when it gets to that subgoal it does it again, in an infinite recursion of procrastination.

3. The goals or actions are vague and meandering, and rather than looping, the agent wanders around talking to itself, accomplishing nothing.

### 