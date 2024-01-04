---
tags:
  - blog
---
 https://threadreaderapp.com/thread/1740051586104295457.html


### System Design Funda

 * It is ok to pick a tech that you had experience with and call out the **pros & cons.**
 * 
### Url Shortener 

Problem statement: Create a short url from a long one.

P0: 
- [ ] Creation of short URL can be delayed by 1-2 seconds.  | 
- [ ] once created, url must be available Always.  | 
- [ ] influencers => Read Heavy system
- [ ] Noun: short URL : Properties - short , unique, fast redirection


Failures: in steps
- [ ] ok. minor - can retry 
- [ ] Nope. No scope for retry

Failures: Core scenarios
- [ ] What if short url does not exist? -- cache bypass 
- [ ] Too many unique urls? => DB overloaded => how to run degraded experience?
- [ ] Revisit URL = browser caching

Hotspots / scale: 
- [ ] Influencers => in memory cache / distributed cache.
	- [ ] scale - sharding -- N strategies of sharding and tradeoffs 
	- [ ] scale - data paritioning 
	- [ ] scale - hashing - which algo? Why? 
- [ ] popular URL - shared by celebrities 
	- [ ] Noisy Neighbours problem -- need to keep them separate to prevent downtime of smaller customers.

Keywords : critical path , availability, fast lookup, consistency

### Distributed systems
https://newsletter.francofernando.com/p/replication

https://careercutler.substack.com/
https://craftbettersoftware.com/
https://www.developing.dev/p/the-developing-dev-wrapped-2023


https://www.developing.dev/p/from-microsoft-intern-to-meta-staff
https://newsletter.techleadmentor.com/p/stop-parroting-youtube-solutions
## CTO 

https://web.archive.org/web/20171128214925/https://juokaz.com/blog/becoming-a-cto

1. Role : Communicating the tech + leading the execution 
2. Responsiblity : 
	1. for mistakes of the team 
	2. how to get the other parts of the company to respect the development processes.
3. “The CTO must protect the technology team from becoming a pure execution arm for ideas without tending to its own needs and its own ideas.”
4. 



## Note Taking 
https://newsletter.francofernando.com/p/retaining-knowledge
1. 









https://huggingface.co/docs/transformers/model_doc/layoutlm
https://huggingface.co/docs/transformers/model_doc/markuplm
https://pyimagesearch.com/2020/08/24/ocr-handwriting-recognition-with-opencv-keras-and-tensorflow/



https://towardsdatascience.com/build-a-handwritten-text-recognition-system-using-tensorflow-2326a3487cd5
https://medium.com/illuin/cleaning-up-dirty-scanned-documents-with-deep-learning-2e8e6de6cfa6


Ayush
- [ ] blockchain
- [ ] 