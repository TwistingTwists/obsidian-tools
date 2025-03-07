---
tags:
  - machine_learning
---


### Retrieval 
2025-03-06 06:36:23
Source: Glean talk (manav rathod)

| step id |                           |                                                                                                                                                                                                                                                                                                                                                       |
| ------- | ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| DSK     | domain specific keywords  | MLM of embedding model (smaller embedding model )                                                                                                                                                                                                                                                                                                     |
| IN_1    | insight                   | even after above DSK, improve perf on search related tasks, <br><br>For search around document:<br>- title-body pairs<br>- anchor data (PageRank over documents => related documents link to each other)<br>                                                                                                                                          |
| QTL     | query types list          | - **Factoid questions**: Seeking specific pieces of information<br><br>- **Enumeration queries**: Requesting lists of items (e.g., "What are all the projects in the finance department?")<br><br>- **Inventory queries**: Looking for assets like meetings, presentations, etc.<br><br>- **Navigational queries**: Trying to find specific documents |
| SDG     | synthetic data generation | give LLM document -> generate QA pairs                                                                                                                                                                                                                                                                                                                |
|         | general improvements      | 1. Generate data - via SDG<br>2. query types - measure perf - QTL                                                                                                                                                                                                                                                                                     |
|         |                           |                                                                                                                                                                                                                                                                                                                                                       |


### Late interactions 
2025-03-06 06:37:17
- 

### Systematically improving RAG 

Making applicant tracking system open source -  more robust as a service

What are the major Issues  in matching the job description with 
- 


Potential videos to watch 
- https://www.youtube.com/watch?v=8e3x5D_F-7c
- https://www.youtube.com/watch?v=cN6S0Ehm7_8
- https://www.youtube.com/watch?v=JNy1v8NLr7M
- https://www.youtube.com/watch?v=EDVqG86AT0Q&t=518s



#### Levels of complexity in RAG
https://jxnl.github.io/blog/writing/2024/02/28/levels-of-complexity-rag-applications/



##### From Wandb course (instructor): 

- [ ] Simple RAG has limitations 
- [ ] howto advanced rag - 
	- [ ] Overview - with evaullation on RAG : https://www.youtube.com/watch?v=CeDS1yvw9E4
	- [ ] Tru and Llamaindex : https://www.youtube.com/watch?v=rrW1U7tt_Xg
	- [ ] more llamaindex : https://www.youtube.com/watch?v=wBhY-7B2jdY
	- [ ] evalutae the rag : https://www.youtube.com/watch?v=mEv-2Xnb_Wk
- [ ] watch people build with MarvinAI 
	- [ ] https://www.youtube.com/watch?v=ku3DPrR6Sl0

Just a question around structured output using instructor 

https://youtu.be/CeDS1yvw9E4?t=745 : This shows the structured output using llamaindex. Instructor produces similar stuff by defining pydantic models. 

1. does llamaindex use instructor?
2. if not, then Is there tangible benefit by integrating instructor with llamaindex? 
3. the integration might be trivial. Or not. If it is trivial, please point to some docs.


### Embeddings 
- [ ] https://huggingface.co/BAAI/bge-reranker-large
- [ ]  What are embeddings ?
