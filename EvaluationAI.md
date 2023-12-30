Scope: 



## Related Data Cleaning 

Get all the pdfs -> extract images -> clean images -> embed text in images?            |
																					-> Segment images and above    |  ==> vectorDb similarity search on it. 
																					-> Embed text directly in pdf.       |
																																			|   


Raj Rajhans - CLIP + text model in Elixir and Bumblebee. 


### Cleaning images and embedding text? 



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



