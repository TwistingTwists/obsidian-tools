---
tags:
  - startups
  - startup-2
---
2025-02-02 10:35:26

### Data ingestion pipeline
Aim: converts the unstructured data to structured

- [x] find the errors in  the data scraping
	- [x] staetment-I 
	- [x] tweak prompt

### 01-pdf-to-text

- [ ] What if you could write a python script for each specific pdf? 
- [x] use docling on two column layout pdfs  - meh. too slow.
- [x] use surya on two column layout pdfs -- with markers already present in the pdf (from surya?)
- [x] extract using existing baml pipeline
- [x] run for SFG_2022 as well 
- [ ] automation
	- [ ] scripts to startup tmux +  json_concurrent_writer + boostrap_meilisearch + 
- [ ] CHECKOUT
	- [ ] https://www.eidolonai.com/docs/quickstart





2025-02-03 09:33:08

W05 

- Ingestion 
	- DSSSB 
	- SFG all 
	- Two column layout paper - vision + vajiram
- Agent 
	- Streaming completion + Jsonish
	- define use cases 
		- report generation - STORM paper stanford
		- PRD = polish it 
		- Coder = use PRD and code


 Streaming completion 
 - [ ] Research Approaches 


- [ ] try out report-generator from langchain-AI  https://github.com/langchain-ai/report_maistro.git
	- [ ] demo is cool. video is nice
	- [ ] Play with it
		- [ ] can you build langgraph using Adjacency list notation? much easier to read. Give the API
			- [ ] NO. too complex. don't want to debug. (it would be cool. But to what end????)
		- [ ] What do you want? -- Agent which can refine my PRDs.
	- 


[[BoxStream in Rust]]


# AI Influencer

Hypothesis: 

2025-02-04 02:05:05
#### Financials
- [ ] 6mo of work - and you can generate 2k / mo reliably! 
- [ ] 1y => $10k / mo
#### Industry 
- [ ] Insta pages / influencers
- [ ] 

#### Krishna, the doer. 

- [ ] Money, ethically.



- [ ] Generate an Influencer 
	- [ ] Name : from google trends 
	- [ ] Figure 
- [ ] Research methods
	- [ ] Flux models which can be trained to yield your own 'variant' of the model
	- [ ] [Kling](https://replicate.com/kwaivgi/kling-v1.6-standard?prediction=bmhg7fpw71rmc0cmt03vkx2pm8) Can 5s video reliably good! 
	- [ ] [Flux Pro Ultra](https://replicate.com/black-forest-labs/flux-1.1-pro-ultra?prediction=wfy9pfkw7xrme0cmt099zj0410) can generate a new image from base image. Do faces match? - nope.
	- [ ] [Stable Diffusion Utlra](https://replicate.com/stability-ai/stable-diffusion-3.5-large) not tried this one yet.


## Linux - STT for typing - purely local
- [ ] SETUP 
	- [ ] uv to setup locally - GLOBAL INSTALLS ? 
	- [ ] should I use dockerfile?