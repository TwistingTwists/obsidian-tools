
- [ ] Scrape PDFs for questions
	- [ ] UPSC PYQ
	- [ ] SFG 2025
	- [ ] SFG 2024
- [ ] insert in sqlite
- [ ] tag questions as per topic / source / theme 



Flow 

- Scrape questions -> tag topic -> 
	- Find other questions similar to given question 
	- Find all questions on the given topic (or closely related topics)


- Mains questions UPSC + test series (related questions) => Topic mapping 
- each topic has set of factoids = {type, content}
	- {article, 60}
	- {judgement,  }


2025-02-21 07:10:23
- student uploads a copy => OCR to get the text => from the text, analyse the copy - find feedback bullet
	- feedback bullet
		- {current_state_of_world, action_to_improve_current_state_of_world}, here state_of_world means the current context
	- 


Process
- Image preprocessing pipeline ([chatgpt_link](https://chatgpt.com/share/67b81046-e730-8010-bea2-b2d00c87708f))


----

### PIPELINE 

2025-02-21 07:10:05
- Image in -> OCR -> text
- analysis
	- demand of the question -- 
	- Intro - data, historical, definition, context (ack the issue in the question, )
	- conclusion - 

Gemini has Grounding Search. It is good. [example](https://aistudio.google.com/prompts/1LuUItFkpkCqh9Y5CjLlE70b04pRPIfZw)

OCR preprocessing steps - https://chatgpt.com/share/67b81046-e730-8010-bea2-b2d00c87708f

Document layout analysis : 
- try sycamore AI - [pip install sycamore-ai](https://sycamore.readthedocs.io/en/stable/)
- [write your own layoutParser ](/home/abhishek/Downloads/experi2/machine_learning/rec_pyq/ai-scraping/deterministic-pdf-scraping-mcq/scratchpad_me.md)
- 


---

2025-02-20 08:51:50

Steps for free credits:

1. signup for google account startup program
	1. get private email  from Namecheap
	2. set dns records at cloudflare (since CF manages DNS)
	3. wait for this to be setup. <<< 
	4. signup on gcp
2. 

---


##### sidecar

- webserver
- qdrant
	- exe, linux, mac
- models - all mini LM v62
- onnxruntime
	- dll, so, dylib
