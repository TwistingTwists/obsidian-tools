
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



- student uploads a copy => OCR to get the text => from the text, analyse the copy - find feedback bullet
	- feedback bullet
		- {current_state_of_world, action_to_improve_current_state_of_world} 
			- {}


----

### PIPELINE 

- Image in -> OCR -> text
- analysis
	- demand of the question -- 
	- Intro - data, historical, definition, context (ack the issue in the question, )
	- conclusion - 

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
