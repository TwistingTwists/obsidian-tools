
2025-02-25 10:10:06
https://claude.ai/share/e590885a-a71c-4512-bf1b-95d8d9799068

##### (todo) PRELIMS QUESTIONS de-duplication: 

|             |                                                                                                                                                                                                                              |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| learn       | <br>https://machinelearningmastery.com/distance-measures-for-machine-learning/<br>https://www.youtube.com/watch?v=e9U0QAFbfLI                                                                                                |
| data-source | `/home/abhishek/Downloads/experi2/machine_learning/rec_pyq/ai-scraping/prod-data/combined.csv`<br>this is the csv                                                                                                            |
| blogs       | do the faiss / qdrant etc to find duplicates , review and remove them.<br>- [ ] found[ the blog](https://minimaxir.com/2025/02/embeddings-parquet/) - embeddings in parquet store => fast dot product to find similarity<br> |


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
	- [ ] caching questions / user response etc - using riverpod, local sqlite as cache ??



