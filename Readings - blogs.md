---
tags:
  - blog
---
 https://threadreaderapp.com/thread/1740051586104295457.html



## Time in software

- [ ] https://www.youtube.com/watch?v=Zm95cYAtAa8
- [ ] https://hexdocs.pm/tempus/getting-started.html
- [ ] https://github.com/wojtekmach/calendar_interval
- [ ] Tempus 

### System Design Funda

 * It is ok to pick a tech that you had experience with and call out the **pros & cons.**

### Url Shortener 

Problem statement: [Create a short url from a long one.](https://newsletter.techleadmentor.com/p/stop-parroting-youtube-solutions)

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


## CDC in Elixir 


| Name      | Value                                                                                                                     |
| --------- | ------------------------------------------------------------------------------------------------------------------------- |
| Popular   | Supabase Realtime, ElectricSQL                                                                                                                          |
| Libraries | walex <br><br>https://github.com/supabase/realtime<br><br>https://github.com/eventrelay/eventrelay?tab=readme-ov-file<br> |
### Staff Software Engineer / EM templates 
https://jordancutler.notion.site/jordancutler/High-Growth-Engineer-Paid-Subscriber-Resources-5927edb323fd4d5fa0ddf86775508586#4d2872788a264f34a8b363fa8ef57b84


https://jordancutler.notion.site/Design-Docs-Templates-15092abc81a34ff79e1bfc4ac6fa3440
[Influence and Incentives](https://jordancutler.notion.site/jordancutler/Influence-Incentives-Buy-in-d6eb2a0855c74758aae2924d34894237)
[Resolving Disagreements](https://jordancutler.notion.site/jordancutler/Resolving-Disagreements-4b8cb95271874a8cbf03458162eb8089)
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

## Leadership in Tech 
https://addyosmani.com/blog/effective-teams/

[Senior Engineer Resources Github](https://github.com/jordan-cutler/path-to-senior-engineer-handbook)


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