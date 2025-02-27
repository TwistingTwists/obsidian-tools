
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



### Steps mobile app for MCQ attempt

2025-02-25 10:10:06
https://claude.ai/share/e590885a-a71c-4512-bf1b-95d8d9799068

##### (todo) PRELIMS QUESTIONS de-duplication: 
`/home/abhishek/Downloads/experi2/machine_learning/rec_pyq/ai-scraping/prod-data/combined.csv`
this is the csv

do the faiss / qdrant etc to find duplicates , review and remove them.
- [ ] found[ the blog](https://minimaxir.com/2025/02/embeddings-parquet/) - embeddings in parquet store => fast dot product to find similarity

v1 release:

- [ ] improvements must have 
	- [ ] formatting for quetsions as markdown - update db 
	- [ ] button on question list page - text is not visible
		- [ ] introduce rapid mode -- you click on the choice and the right / wrong is evaluated instantly. No need for submit button anymore
	- [ ] filter by topic, 
		- [ ] list of all topics from supabase
	- [ ] store user responses in supabase (phase 2 and phase 3)


- [x] SFG 2025 scraped data has been put into supabase `prelims_questions`
	- [ ] may have duplicates
- [x] serve this from mobile app - paginated 10 questions at a time
- [ ] add PYQs of upsc exam
- [ ] filter by wrong attempts
- [ ] improvement nice to have 
	- [ ] caching quetions / user response etc - using riverpod, local sqlite as cache ??





### Steps for mobile app - pdfviewer with ripgrep-all search 




### Steps for Cargo rust flutter app - syllabus

Basic code plan 
- https://chatgpt.com/share/67bb5036-4268-8010-b022-05a0c1fb9ecb
- https://chatgpt.com/share/67bb5056-296c-8010-a4fa-01abd1d626f2
- https://chatgpt.com/share/67bd4b5a-6b40-8010-a4fb-a8ff1328a9b2

#### Mains questions - database
- questions from diamond could be put in supabase as starters ? (folder: ingest_meili)

- [x] topic_tags have been put in supabase as nested recursive table (folder: ingest_meili)
- [ ] 



Socials 

- review of whoop app -- I am rank 9 / 10k users who use whoop. 
- if I do this, then my sleep goes for toss. I have data to show.


### PIPELINE for mains questions database



### PIPELINE for analysis

2025-02-21 07:10:05
- Image input 
	- [x]  OCR 
	- [ ] clean text of OCR
	- [ ] text continuity analysis
	- [ ] extract question, extract answer (text - question)
	- [ ] 
- analysis
	- demand of the question -- 
	- Intro - data, historical, definition, context (ack the issue in the question, )
	- conclusion - 

Gemini has Grounding Search. It is good. [example](https://aistudio.google.com/prompts/1LuUItFkpkCqh9Y5CjLlE70b04pRPIfZw)

OCR preprocessing steps - https://chatgpt.com/share/67b81046-e730-8010-bea2-b2d00c87708f

Grouping pages into questions workflow 
- https://chatgpt.com/share/67bab51d-22d8-8010-9b38-1b8e70609052


Document layout analysis : 
- try sycamore AI - [pip install sycamore-ai](https://sycamore.readthedocs.io/en/stable/)
- [write your own layoutParser ](/home/abhishek/Downloads/experi2/machine_learning/rec_pyq/ai-scraping/deterministic-pdf-scraping-mcq/scratchpad_me.md)






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


### FineTune

Resources 
 - https://app.openpipe.ai/p/A3skFhW8ph/request-logs 

Funda 
- evals + dataset => can finetune

---

##### sidecar

- webserver
- qdrant
	- exe, linux, mac
- models - all mini LM v62
- onnxruntime
	- dll, so, dylib
