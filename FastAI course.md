---
tags:
  - nlp
  - qna
  - python
  - machine_learning
  - study
---
## LLM Steering course 
https://www.wandb.courses/courses/take/steering-language-models/lessons/51841451-jason-s-introduction

HuggingFace NLP
https://huggingface.co/learn/nlp-course/chapter1/1

#### Google's mediapipe 
https://developers.google.com/mediapipe/solutions/setup_web


### Fine tuning model for function calling
https://parlance-labs.com/education/fine_tuning/pawel.html

### resources for question answering

https://github.com/ramsrigouthamg/Questgen.ai
https://huggingface.co/course/chapter7/7?fw=pt
https://www.youtube.com/playlist?list=PLIUOU7oqGTLjWyG832fMAcBYfyRZvU_9-
https://www.youtube.com/watch?v=X05uK0TZozM&ab_channel=JamesBriggs
https://www.youtube.com/watch?v=OQhoi1CabWw&ab_channel=Pinecone



## Prompts list for coding
https://github.com/0xpayne/gpt-migrate/blob/main/gpt_migrate/prompts/p4_output_formats/multi_file


## FastAI course
#### L4

1. Transformers
  Chunks -- deleted words predict.
  Masked language model

2.Going from a model taht can predict the next word to a model that can classfiy ?

3.RestNet50 --
  early layers - almost the same
  later layer more specific to training task.

4. BERT has certain vocabulary. How to train a base model to increase the vocabulary?
  patent vocabulary vs upsc vocabulary -- building vocabulary?
  good to know what doesn't work for you

5. good training / validation set split. because ML model may find correlations that don't exist.
  > looking for specific fish? -- it may correlate it to boat. :p then, seeing boat, it says event N captures more fish because of boat. Which is wrong.

  Random test train split.

6. Loss : model is being trained on minimising loss.
  Score : you actually care about the metric in real life.

  Model -

7. Pearson correlation coefficient
  R = -1 => wrong answer
  R = 1 => right answer

  if the evaulation metric is correlation coefficient , then you need to make sure that outliers are removed by visualising data. [for more](https://youtu.be/toUgBQv1BT8?t=4276)

8.
