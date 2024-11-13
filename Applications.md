

Edtech  - Eigen - TS 
https://jobs.ashbyhq.com/eigen/9ee4086c-b36c-493a-9d65-42c5d26cca9b




What are Branded Companies you want to go to?
1. victoria metrics 
2. Metabase - [Backend Role](https://jobs.lever.co/metabase/85f454d8-e795-4978-8a2b-4b8bfa7d7c37?lever-origin=applied&lever-source%5B%5D=HackerNews) 
3. 

Not tier 1, but super interesting 
##### foxglove
- [RustRole](https://foxglove.dev/careers-single?id=5187250004)  : delivering streaming and analytic workloads for robotics data.
	- binary data serialization or network communication protocols
- [Staff Frontend Role](https://foxglove.dev/careers-single?id=5348223004) - performance sensitive Frontend Engineering 
- 


Industries to familiarize yourself in: 
1. logisitcs - warehousing - 
2. robotics / etc data streaming
3. ad / marketing
4. LLM


UpSkilling 
- Kubernetes familiarity
- data infra = golang / tech + landscape (blogs)
- LLM 
	- evals setup
	- prompt arrangement / setup / Cursor opensource react style context window management library?


Eigen 

- What other AI tools have you experimented with?

instructor - python , ragas for evaulation, vapi.ai / play for voicebots , sileroVAD for speaker detection, InVideo AI editor

- What education or consumer-facing products have you worked on?

mathocrat.com = it is an LMS (learning management system) that helps educators deliver on-demand paid courses to student. Think of Udemy for educators.
Tech Stack used: Redis, Elixir, background job processing, distributed video processing pipeline (ffmpeg)

pmf_quiz_bot = telegram bot that automatically generates a quiz based on the news of the day to improve retention among students. Currently used by PMFIAS org to drive retention focussed quizzes and news articles.
Tech stack used: playwright, Elixir, postgres, openAI with Cursor to quickly write SQL queries and drive insights in user behaviour

data extraction and formatting from pdfs = given a set of pdfs, extract (GPT-4o) the multiple choice questions, make structured json (Haiku with instructor python library), and then use for quiz bots 

data cleaning = given a report of all new startups of a related domain, present the structured output format of the key details available in the document - Founders, company size, expertise area, funding, short description, products, business model category (SAAS, B2B, B2C, consulting). Augment the missing data from news articles about the startup from the internet. e.g. recent funding , if any.


- Bonus: Describe how you've used OpenAIs or Anthropic's developer products.

 A. embeddings =  `text-embedding-3-large` for generating embeddings for text
 B. Vision API = OCR for handwritten text, table detection (table-transformer too!), 
 C. Whisper API = for live transcriptions and long form transcriptions from pre-recorded audio files
 D. instructor = for structured output generation
 E. ragas = for creating evaluation harness
 F. prompt caching with Antrhopic = Improving latency and costs in repeated prompts related to a document question answering
 G. AI agent based workflows = GPT-4o as the main 'brains' for data extraction and formatting workflow. Data extraction via GPT-4o and Claude Haiku as the secondary agent for data formatting tasks (low intelligence).




data 

- Streaming / batch = ingestion + querying (db / linux syscall)
- Data engineering = Advanced in ETL and Postgres SQL, data orchestration with dbt/Dagster