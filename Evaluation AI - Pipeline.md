
### Task LIst 

- [ ] Scrape PDFs for questions
	- [ ] UPSC PYQ
	- [ ] SFG 2025
	- [ ] SFG 2024
- [ ] insert in sqlite
- [ ] tag questions as per topic / source / theme 

2025-02-23 07:56:02
- [ ] setup experiments repo for evaluationAI
	- [ ] image preprocessing experiments
		- [x] to cleanup image before doing OCR (contrast up+ binarization)
			- [ ] need better approach than this. next step: try restormer, and 
			- pre-Process
				- Image preprocessing pipeline ([chatgpt_link](https://chatgpt.com/share/67b81046-e730-8010-bea2-b2d00c87708f)) -
				- papers and  other models https://www.one-tab.com/page/wEArOK3xTDSBBwLVqGL30w
					- https://github.com/swz30/Restormer?tab=readme-ov-file has 30-60 MB models for various image cleaning tasks. Maybe look at these for  preprocessing
		- [ ]  crop in a bounding box for OCR
	- [ ] OCR
		- [x] instructor library is shit. Baml is much better. using Qwen 7B for OCR
		- [x] b01_ocr.py file -- extracts text 
		- [x] pdf -> images -> extract text -> group text into a single question -> track metadata like page_number
		- [ ] next step: clean each question text (contains junk words/ keywords etc etc ) -> page grouping algorithm
	- [ ] ~~OCR based grouping workflow (which pages form part of one question)
		- [ ] printed questions at top
			- [ ] also has text in margin - crop in a bounding box for OCR
		- [ ] handwritten questions at top
		- [ ] NO questions, only answers
	- [ ] DATABASE for all variations of input question - pdf scraping for mains questions - put in sqlitedb
		- [ ] from initial database of UPSC PYQ + scraped from test series - second order thinking, first order thinking etc questions generate. (with reasoning traces to be stored in sqlite so that samples can be used for dataset generation)
		- [ ] topic tagging - nested topics (maintain hierarchy during tagging) + multiple topics
	- [ ] ingesting unstructured sources
		- [ ] ArticleProvider (web search for latest data on internet) 
			- [ ] perplexity - on a given topic, it gives me lots of references I can click and follow
			- [ ] gemini - with grounding search 
			- [ ] deep research OpenAI
		- [ ] DataSet provider
			- [ ] SunyaIAS pdfs?
			- [ ] vision ca magazines
			- [ ] forumias mains test series
			- [ ] forumias topper copies
		- [ ] 


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


----

| type | link                                                              | description |
| ---- | ----------------------------------------------------------------- | ----------- |
|      | [[Steps mobile app for MCQ attempt]]<br>                          |             |
|      | [[Steps for mobile app - pdfviewer with ripgrep-all search ]]<br> |             |
|      | [[Steps for Cargo rust flutter app - syllabus]]<br>               |             |
|      | [[pipeline - Mains questions]]<br>                                |             |
|      | [[PIPELINE - student copy analysis]]<br>                          |             |
|      | [[FineTune LLM models]]                                           |             |






