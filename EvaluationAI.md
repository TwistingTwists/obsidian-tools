


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






## Elixir and Evaluation AI 
Pydantic is all you need :  https://www.youtube.com/watch?v=yj-wSRJwrrc
https://github.com/thmsmlr/instructor_ex
https://www.youtube.com/watch?v=YrRmN4LbCuE -- Elixir recommendation engine

https://github.com/clovaai/deep-text-recognition-benchmark?tab=readme-ov-file






PMFIAS client 

| Step | yes | no | Tech Implementation plan  |
| ---- | ---- | ---- | ---- |
| Are they launching prelims test series? | How we can add value?<br><br>- Give test portal -- reminders <br>- Upload from world document or csv file directly.<br>- Resumable test <br>- Exam results publishing <br>- Multiple attempts<br>- PYQ suggested from UPSC | Mobile app bana do<br>- managing<br><br>6k per month for 400 users.<br> |  |
| PYQ UPSC test series | - free? = yes<br>- next year prelims test series with us<br>- students sign up - user base / user interaction <br>- student data<br><br><br><br>- Highlights text and text editor -- to easily makes notes from any test series and export them to pdf. <br><br> |  | * Pandoc for importing from word doc |
|  |  |  |  |



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