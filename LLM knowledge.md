---
tags:
  - llms
  - interview
---

engineer -> developer
bankers -- capital flows 



What has been your journey? how have you been making decisions? - get good. enjoy stuff.

What are you r motivations for the role. from here on?


IT services - IR 

go on end client projects -- run discovery -- come back with proposals -- speed up delivery + quality up

building tooling support for project + owning / acceleration 

Generalist role == 
Enterprise sales shit take

playbook / runbooks == 

tonaliy of a chatbot == role / skills 

agent design , write a JD, 

high leverage work , analytics report , 

strong held 




role 
- sales - alignnig stakeholders
- forward deployed 


night and day -- llm by night 

Tell me about yourself:

- did a  startup in past. ProjectIAS 
- full stack roles across languages - elixir, nodejs , rust (current)
- Tech experience - backend api (elixir), kafka, distributed elixir, 
- team lead at tinymesh
- Mathocrat --  udemy like VOD

- past 1 year
	- basics - coursera course on ml
	- made a RAG over legal documents - not performed as well I would like it to. early days
	- image -> question -> analysis 
- AI side projects
	- evals = 
	- forever vm - runsc gvisor based vm for isolating workloads
	- project manager cli 


What does the next carrer trajectory look like?


Todo:


1. [recommendation systems - eugene yan ](obsidian://open?vault=learnings&file=recommendation%20system%20at%20scale)
2. structured outputs - curator 
	1. llm chaining 
	2. PEG grammars for structured outputs - sglang + llama.cpp (context-free grammars) = Constrained generation
	3. prompt management via jinja templates
		1. prompt and curated dataset versioned together
3. Hands on LLM book 
	1. tokenizer
	2. attention mechanism 
$$
\text{Attention}(Q, K, V) = \text{softmax}\left( \frac{QK^\top}{\sqrt{d_k}} \right) V
$$
	3. temprature 
	4. KV Cache, 
	5. [positional encoding](https://www.youtube.com/watch?v=T3OT8kqoqjc) - deterministic sine / cos wave functoins over 768 dimensions
	6. SFT / RL 
4. Other basics
	1. retrieval vs ranking

5. RAG
	1. evals
6. Misc 
	2. mitm - show me the prompt. doesn't work for cursor
7. New ideas you are excited about
	1. Late interaction (omar khattab) - embed first, chunk later
		1. colipali / Qwen VL
	2. Curator - synthetic data generation from LLMs -- reasoning datasets
	3. fine tuning (see below)
	4. 
8. Fine tuning
	1. when to do LoRA vs Full Fine Tuning?
9. MCP
	1. good for tools, need resource reference in MCP
		1. So that entire trasncript  / audio etc is not sent over the wire etc.
	2. 
10. Agents
	1. When to build an agent? ANthropic


System Design for Applied LLMs:
1. design deep search  (perplexity)
2. design kapa.ai - website search

Retrieval deep dive
re-ranking deep dive




favourite blogs you read?
fav books?
fav authors?
- hamel 
- eugene yan - rec sys 
- guest lectures organised on blog of jason liu - glean
fav products?
- kapa.ai - retrieval is super solid 
- ai news- swyx


## RAG 

[[Embeddings]]
##### Query Pattern Recognition[¶](https://jxnl.co/writing/2025/03/06/fine-tuning-embedding-models-for-enterprise-rag-lessons-from-glean/#query-pattern-recognition "Permanent link") 

An important aspect of embedding fine-tuning involves recognizing different query patterns. As mentioned during the discussion, queries often fall into categories like:

- **Factoid questions**: Seeking specific pieces of information
- **Enumeration queries**: Requesting lists of items (e.g., "What are all the projects in the finance department?")
- **Inventory queries**: Looking for assets like meetings, presentations, etc.
- **Navigational queries**: Trying to find specific documents

## LLM as judge 

Tips: 
write critiques - let it reason
diverse examples -- features, scenarios, personas
error analysis = errors across scenarios - further breakdown into ErroType => mark as percentage - tackle the most occuring error first

| Title | Authors/Organization | Type | Key Insights | Citation |
|-------|----------------------|------|--------------|----------|
| From Generation to Judgment: Opportunities and Challenges of LLM-as-a-judge | Papers With Code | Survey Paper | Introduces taxonomy for LLM-as-a-judge across three dimensions (what/how/where), emphasizes need for domain-specific benchmarks, and identifies challenges like bias mitigation and hallucination control. | [1] |
| Best Practices For Creating Your LLM-as-a-Judge | Galileo AI | Blog Post | Advocates for discrete evaluation scales (e.g., boolean/categorical) over continuous scores, stresses importance of structured output formats (e.g., JSON), and recommends hybrid human-AI review systems. | [2] |
| LLM-as-a-judge: A Complete Guide to Using LLMs for Evaluations | Evidently AI | Technical Guide | Demonstrates 80% agreement between GPT-4 and human evaluators in pairwise comparisons, highlights cost savings over crowdsourcing, and proposes iterative prompt refinement for evaluation criteria. | [3] |
| The Definitive LLM-as-a-Judge Guide for Scalable LLM Evaluation | Confident-ai | Blog Post | Categorizes evaluation methods into single-output scoring (with/without references) and pairwise comparisons, warns against flaky judgments due to non-determinism, and introduces deterministic metrics via frameworks like DeepEval. | [4] |
| Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena | Zheng et al. | Research Paper | Reveals GPT-4 achieves >80% alignment with human preferences, identifies position/verbosity biases in LLM judges, and validates MT-Bench as a multi-turn evaluation benchmark. | [5] |
| What is LLM as a Judge? How to Use LLMs for Evaluation | Encord | Blog Post | Proposes LLM judges reduce evaluation costs by 70% compared to human reviewers, discusses AI alignment challenges in judicial contexts, and emphasizes data quality improvements via tools like Encord. | [6] |
| Humans or LLMs as the Judge? A Study on Judgement Bias | Chen et al. | Conference Paper | Exposes vulnerabilities to misinformation/gender/authority biases in both human and LLM judges (e.g., 35% error rate under adversarial attacks), questions reliability of current evaluation paradigms. | [7] |
| LLM-as-a-Judge Evaluation for GenAI Use-Cases | Arize AI | Blog Post | Links LLM judges to business metrics (e.g., purchase rates), advocates for synthetic benchmark generation, and addresses scalability gaps in human feedback collection. | [8] |


## MCP 
MCP servers can provide three main types of capabilities:

1. **Resources**: File-like data that can be read by clients (like API responses or file contents)
2. **Tools**: Functions that can be called by the LLM (with user approval)
3. **Prompts**: Pre-written templates that help users accomplish specific tasks

## Constrained Generation 
### PEG for json

```lark
?start: value

?value: object
| array
| ESCAPED_STRING
| SIGNED_NUMBER      -> number
| "true"             -> true
| "false"            -> false
| "null"             -> null

array  : "[" [value ("," value)*] "]"
object : "{" [pair ("," pair)*] "}"
pair   : ESCAPED_STRING ":" value

%import common.ESCAPED_STRING
%import common.SIGNED_NUMBER
%import common.WS

%ignore WS
```

### PEG / CFG / Recursive descent parsers

A **PEG** is like a _refined_, deterministic version of a CFG that is designed to be directly **parsed using recursive descent**.

> **Recursive descent is a _natural fit_ for PEGs.**

Why?

- PEGs define exactly **one way** to parse a string — the first rule that matches wins.
    
- This removes ambiguity, making recursive descent simple and predictable.

|Concept|Purpose|Fits with...|
|---|---|---|
|**CFG**|Theoretical model of syntax|Can be used with various parsers|
|**PEG**|Deterministic grammar for implementation|Maps cleanly to recursive descent|
|**Recursive Descent**|A parser technique (top-down)|Can implement PEGs or LL CFGs|