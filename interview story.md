---
tags:
  - interview
---

- focus on " I "
- engineering process tell 
- my involvement -- lead , stakeholders, distribute , delegate 
- dollar conversion + engineering time saved 


### Most challenging project you ever did

- staff - cross team collab 
- SDE-3 - Deep Tech Challenge + little team collab
- SDE-2 - library likha


- WHAT - designing pubsub system with event bus in rust
- WHY
- STAR
	- payment / order notification system
	- notification -- notify other microservice / notify user
	- A: MVP users projection 1yr + 10x spikes => 
		- library + integration tests + load test
	- integrated into our codebase
	- payment / order async + features - delivery email / delivery ads 
- People : team 3 - 10x  -- 
	- MVP 3 ways -> load test 
 
- Staff 
	- marketing team - growth projection => major airdrops -- traffic spike 
	- figure out other features that can be built using same lib --- invest in this library
	- Impact : 
		- TECH -- 10000 signup a day , same infra -- 10x 
		- BUSINESS -- marketing team 1 aidrdops == 3 -- 100 => 10000 signups a day


wide scope  + one week + scope and plan / collab + design for 10x + 


LE = learning and enablement 

Learning 
- new joinee - less context , speed up to the pace , 
	- talk to the person -- 
		- delegate the task which is fit for the person
		- talk to manager, assign on a different team
		- understand motivation and align with team + values + ambition
	- Why this project? = project groomed to be a better leader -- human errors handling, due to external factors

- bring to the speed + org cutture + team ambition working style 
	- observe = async (PR review all)  + sync (load testing setup -- )
	- small task - hands on 
	- buddy assign - ask freely questions around the process - async communication -- helped us evaluate him -- 
		- set you up for success =  do well with us in next 6 months



Enablement: 

### situation where you felt like a professional failture.

react + learn + improve 


##### Steps:
realise that it is a failure
feedback

What has happened?
- Technical failure 
- Team failure - lost my position as an expert , loss of trust with other teammates, 
- Grateful - ( Not blame culture ) 
	- team helped me with feedback / 
		- generate actionable insights for me 
		- for the team as well -- incident playbook for that specific scenario
			- commitment to daily / weekly action
			- commitment to measurabe metric 

react = growth mindset + actionable insights + clear commitmment to action with the team (trust building)


### Technical professional failure

> High standards - codebase should be left in a position better than we found it

Slack - We openly communicate to commitment to standards that we uphold - teams, individuals. Company.

Team: 
- codebase better. 
- maker checker = review is must. (infra teams) -- translated to high ownership -- infra outage => rollback by those two folks

soft 
- Planning time reduce + oversight => logging infra + tech debt => oncall  
- Remedy -- quick fix the logging library 

soft 2
- Devlogs left in prod => cleanup not done properly => datadog bill apna shoot 



Hard 
- Cache invalidation
	- Subscriptions not deleted in cache, but expired from db
- Read path - cache.  write path - CUD , subscription - expiry date (caching invalidate)
- Find out about it? -- monthly subscriptions but kept increasing, renewable revenue is not increasing same about

Pair it with some edge case. 


hard 2
- Deleted the db + thankfully it was staging



### feedback to seniors 

managers 






















You have to show that you love the craft!!! 



### 🔹 4. **Behavioral & Leadership Interviews ("Googliness")**

This round assesses leadership, ownership, collaboration, and how you fit Google's culture.

#### Focus Areas:

- Project ownership: initiative, leading efforts
- Mentoring juniors or cross-functional collaboration
- Conflict resolution
- Dealing with ambiguity
- Balancing quality vs speed
- Learning from failures
#### Framework:

Use **STAR (Situation, Task, Action, Result)** to structure answers.

Prepare stories for:

- Leading a project end-to-end
    
- Overcoming a challenging technical problem
    
- Disagreeing with a teammate or PM
    
- Making a decision with incomplete data
    
- Mentoring or unblocking someone



### Management 

| question                                 | breakdown                                                                           | story 1                                                                                                                                                                                                                                                                                                                                                    | story 2 |
| ---------------------------------------- | ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| What has been your greatest achievement? | achievement<br>(transferrable skill, teamwork)<br><br><br>Greatest (impact)<br><br> | S: Validation + real time feedback <br><br>T: Devise solution<br><br>A: Convincing my boss to bet on the Elixir LiveView which would give us return 6 months later. MVP + training plan<br><br>R: Delivered in 3mo + landed client<br><br><br>_It also taught me how cross-functional collaboration can turn a technical fix into a product-wide win._<br> |         |
| Name a time you stepped up               |                                                                                     |                                                                                                                                                                                                                                                                                                                                                            |         |



[[invideo]]

[[behavioural template]]



I have built state of the art whenever I have 
- encrypted video 
- upload upto 5GB resumable SOTA 
- SOTA - resumable transfer connection 
