---
tags:
  - startups
  - startup-2
---
### Pick best model?

- [ ] https://txt.cohere.com/struggling-to-pick-the-right-ai-model-lets-break-it-down/
- [ ] 
### Have a look 
- [ ] CLIP model hugging face - https://huggingface.co/BAAI/EVA-CLIP-8B
- [ ] LLM modulo framework - https://arxiv.org/pdf/2402.01817.pdfa

[[structured thinking]]

### Embeddings 
- [ ] https://huggingface.co/BAAI/bge-reranker-large
- [ ]  What are embeddings ?
- [ ] 



## Attention is all you need 
- [ ] https://www.youtube.com/watch?v=bCz4OMemCcA
	- [ ] Tokenizer - [Karpathy](https://www.youtube.com/watch?v=zduSFxRajkE&t=1s)
	- [ ] [What is QKV in tokenizer?](https://2020machinelearning.medium.com/decoding-the-key-query-value-mechanism-in-transformer-models-thru-a-deep-discussion-with-claude-ai-2da5a66d4f8e)
	- [ ] 

## Deno_EX - OCR in elixir 
https://github.com/akoutmos/deno_ex

## LangGraph 

https://www.youtube.com/watch?v=hvAPnpSfSGo

https://github.com/samwit/langchain-tutorials
https://github.com/samwit/llm-tutorials

### LLM Course 
Source: https://github.com/mlabonne/llm-course


### Vision Transformers Pytorch 
https://github.com/lucidrains/vit-pytorch#vision-transformer---pytorch


### Building Advanced RAG 

-----
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
4. 

-----
##### LangGraph and CRAG 

- [ ] CRAG - https://github.com/mistralai/cookbook/blob/main/langgraph_crag_mistral.ipynb
- [ ] LangSmith - stacktrace



-----

##### LLM aware 

https://github.com/llmware-ai/llmware


-----

##### Embeddings 

- [ ] https://supabase.com/blog/matryoshka-embeddings
- [ ] 
-----

[[Advice on startups]]

## Pytorch 

https://www.youtube.com/playlist?list=PLZbbT5o_s2xrfNyHZsM6ufI0iZENK9xgG
https://www.youtube.com/playlist?list=PLQVvvaa0QuDdeMyHEYc0gxFpYwHY2Qfdh

### ROADMAP 

1. Data Cleaning 
	1. Get all the pdfs -> extract images -> store related metadata in sqlite database or csv format. 
		1. metadata - topper_name, year, topic, subject {gs , optional name }
	2. make the process reproducible
2. Image to text 
	1. gpt-4 image to text and make csv
3. {optional} find a way to segment images and store the text of the image along with segment info
	1. question
	2. answer
	3. related_data
	4. related_diagrams
4. similarity search 
	1. text to embeddings 
	2. data model - embeddings, image_path, text
5. 


 

Raj Rajhans - CLIP + text model in Elixir and Bumblebee. 


### Image to text 

https://dev.to/ndrean/an-example-of-semantic-search-with-elixir-and-bumblebee-h9n
https://hexdocs.pm/bumblebee/examples.html
https://www.youtube.com/watch?v=ELBQytOaQUQ&t=1s - seanmor5 doing bumblee support chat bot 


## { startup-2 } Segment image into questions and solutions

Aim: Given a prelims test pdf, extract all question as images and present them to the user with answers (as images). Also, give the options as telegram options to the user. 



- [ ] Present the questions as series of 10 / 20 -- followed by solutions 
	- [ ] Decipher the correct answer by parser combinator. -- editable on the liveview. (use AshAdmin for these resources)
- [ ] Classify the question in given categories. -- Predict One time + Human Editable in LiveView.
- [ ] Reminder - Make them revise it as well.


### Technical Architecture: 

#### User Flow : 

User uploads a pdf -> find all the question images. correct answers. answer images. (with OCRs? or searchable text embedding corresponding to each image)



## { startup-3 } Similarity search with topper copies.

#### User Flow: 

User uploads his scanned copy (pdf, image) -> find the text -> find similar topper answer


- [ ] find the text
	- [ ] find the question 
	- [ ] find the answer 
	- [ ] guess the topic / gs

- [ ] find similar topper answer
	- [ ] From same topic / similar topic or similar question from database
	- [ ] Subsequent filters - topper name, year, institute 



### Creating structured data from topper copies 

- [ ] Split a pdf into 1 pdf per answer 
	- [ ] embed the topper name, year, institute in the pdf name, topic?
- [ ] per pdf, find - 
	- [ ] question and answer text - tesseract + classification
	- [ ] topic from syllabus 
	- [ ] nearest PYQ of UPSC (top 3) -> mains
- [ ] 






## Elixir and Evaluation AI 
https://www.youtube.com/watch?v=YrRmN4LbCuE -- Elixir recommendation engine

https://github.com/clovaai/deep-text-recognition-benchmark?tab=readme-ov-file


## PMFIAS client 

| Step | yes | no | Tech Implementation plan |
| ---- | ---- | ---- | ---- |
| Are they launching prelims test series? | How we can add value?<br><br>- Give test portal -- reminders <br>- Upload from world document or csv file directly.<br>- Resumable test <br>- Exam results publishing <br>- Multiple attempts<br>- PYQ suggested from UPSC | Mobile app bana do<br>- managing<br><br>6k per month for 400 users.<br> |  |
| PYQ UPSC test series | - free? = yes<br>- next year prelims test series with us<br>- students sign up - user base / user interaction <br>- student data<br><br><br>: LONG TERM GOAL : <br><br>- Highlights text and text editor -- to easily makes notes from any test series and export them to pdf. <br><br> |  | * Pandoc for importing from word doc |
| Support Telegram bot -- FAQ support bot  | <br>Responds with pre-prepared answers. FAQ prepared by PMFIAS<br><br> |  | Tech :<br><br>- Intent classfication<br>-   |
| meili search on video / pdf content |  |  |  |
PMFIAS client --- 

1. Share and divide the company with tech ancillary 
2. Integarte with pmfias -- for a fee.
3. Cannot share the stake of pmfias 


- testpress prelims courses 
- Video Class Series + prelims test series 

PmfIAS -- Content creator only. now. moving to 'experience' stage

1-2 years = Prelims years

1. prelims 
2. video content 
3. mains test series.


2024-01-23 12:38:16

Agenda : 

2. prelims test series portal 
3. telegram bot 
4.  meili.saraswatielixir.dev


## { startup-4 }  Structured learning prompts for students. 


Aim: Help remember more. By doing structured thinking. Probing thinking.

Literature: 
1. What is [[structured thinking]]?
3. Is it proven to be effective?
4. Can you gamify it?

Keyansh - can he learn table of four? 


## Deep Learning Tutorials 

- [ ] https://www.youtube.com/watch?v=N-AuM3_8IrA -- hadnwritten text using pytorch 
## Literature Review on HTR (handwritten text recognition)

#### Here are a few ways you could adapt ResNet for handwriting recognition:

1. **ResNet + LSTM**: Use ResNet to extract features from the image, then feed these features into an LSTM to capture the sequential nature of handwriting. This could be particularly useful if you're dealing with cursive handwriting where the order of the strokes matters.
    
2. **ResNet + Attention**: Use ResNet to extract features, then use an attention mechanism to weigh the importance of different parts of the image. This could help the model focus on the most important parts of the handwriting.
    
3. **ResNet + CTC (Connectionist Temporal Classification)**: Use ResNet to extract features, then use a CTC layer for sequence prediction. CTC is often used in tasks like speech and handwriting recognition where the alignment between the input and output is unknown.
    
4. **ResNet + Dense Layer**: Use ResNet to extract features, then add a dense layer (fully connected layer) at the end for classification. This is a simple and straightforward approach, but it might not work as well if the handwriting is cursive and the order of the strokes matters.
    
5. **ResNet + Transformer**: Use ResNet to extract features, then feed these features into a Transformer model. The Transformer model, which was originally designed for natural language processing tasks, has been shown to work well on image-based tasks as well.

#### 



## { startup-4 } Micro Saas - Credit based billing system 

Elixir Apps



### Can You retrain llamacpp for your own work ? and let it run on mobile? 

* Old customised model for your domain and your use case. 
* Build the training data for image analysis using vision models / old models  for your use case?



### 30 days of AI 
 
https://docs.google.com/document/d/1GahYgJ-LEICZBJ0_-iBc_k-4_i15pQz3_dCBaRi2nmU/preview#heading=h.wfz67hmysyoo

