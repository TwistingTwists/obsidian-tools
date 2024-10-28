---
tags:
  - study
due: 2024-11-10
---


[Art of Multi processor programming ](https://www.youtube.com/playlist?list=PLbsY-4I8oat9o7p4re3308L4uk0YJe8ez)


### Video 001 

- Describe behaviour of concurrent object and check if that description is CORRECT

###### 001 - Design Concurrent Queue

way01 
- circular buffer 
- lock -- single shared lock for all values
- pointer to head and tail 

way02  - wait-free lock-free - two threads - one enqueue, other deq
- What does QUEUE mean?
- Is this CORRECT?
	- specify in terms of state, which captures the interaction between two thread

Concurrent Specs for way02 - foundations
- Methods
	- take time
- Docs
- adding new methods

### BIG question
- is the concurrent object CORRECT?
	- what IS FIFO queue
		- FIFO means strict temporal order
		- concurrent means ambiguous temporal order
- Lineariazability 