---
tags:
  - llms
  - interview
---

todo:

1. JasonLiu stuff 
2. recommendation systems - eugene yan 
3. structured outputs - curator 
	1. llm chaining 
	2. PEG grammars for structured outputs - sglang + llama.cpp (context-free grammars) = Constrained generation
	3. prompt management via jinja templates
4. Hands on LLM book 
	1. tokenizer
	2. temprature vs 
	3. KV Cache, 
	4. SFT / RL 
5. Other basics
	1. retrieval vs ranking
	2. 
6. RAG
	1. prompting = CoT / PoT / ToT
	2. evals
7. Misc 
	1. mitm - show me the prompt. doesn't work for cursor
8. New ideas you are excited about
	1. Late interaction (omar khattab) - embed first, chunk later
		1. colipali / Qwen VL
	2. Curator - synthetic data generation from LLMs -- reasoning datasets
	3. fine tuning (see below)
	4. 
9. Fine tuning
	1. when to do LoRA vs Full Fine Tuning?
10. MCP
11. Agents
	1. When to build an agent? ANthropic


System Design for Applied LLMs:
1. design deep search  (perplexity)
2. design kapa.ai - website search

Retrieval deep dive
re-ranking deep dive




favourite blogs you read?
fav books?
fav authors?



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