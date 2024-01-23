---
tags:
  - freelance
---


2024-01-21 11:16:22

* first meeting done -- feedback is more TDD.
* second meeting done -- progress is good 
* third meeting due 


### Knowledge and Insights
  
* World -> (  >=1 SUCT )
* WH -> Tank (1 UCT + n CT)  + Pool (>=1  of any type {FP, CP, UCP} )
* UCT -> SUCT or UCP


World 


Tank::UCT = uncapped
Tank::S = standalone (can exist outside a warehouse) 
Tank:SUCT => standalone uncapped tank



## Plan 

Going for graph approach, here are the broad steps and timeline.

- [ ] Create data models ( basics )- world, warehouse, pipe, pool, tank = 1 day
- [ ] API design and validations: Distinguish data models with types - Feeder Node , Pool types (FP, CP, UCP), Tank ( SUCT, etc , Regular CT) with given constraints  = 1 day
	- [ ] Make the graph implementation flexible =  2-3 days
	- [ ] Test them = 3-4 days iteratively testing and fixing corner cases.



TDD - 1 test / 2 tests

- [ ] Momentum + getting rid of unnecessary tests + test have one reason to pass / fail 
- [ ] Miss out - refractoring everything as you go.
- [ ] 


--- Elixir idiomatically, exunit , in memory state management, 


- [ ] Test Euler Method and circularity = 3-4 days

- [ ] Classificiation for determinate WH = 1 day (API design , questions , 1-1s if needed)
	- [ ] Testing -- 3-4 days with iteratively fixing corner cases.
- [ ] Classificiation for indeterminate WH  = 1 day
	- [ ] Testing -- 3-4 days with iteratively fixing corner cases.

Assumption is 3 hr a day.

### Approaches for the circularity module

1. Ecto based (using Ash which enables process based  {ets based}  persistence)
	1. advantage : 
		1. using well known and flexible SQL to write queries and associations => can add complex rules arbitrarily 
	2. disadvantage: 


2. Building Graph and using graph algorithms to check circularity 
	1. 1. advantage : 
		1. can directly use well known algorithms to check circularity
	2. disadvantage: 
		1. have to think about persistence (jsonb, schemas or otherwise)



3. preparing our own dsl based on constraints and rules (using Ash)
	1. advantage : 
		1. easier to use common vocabulary to write new rules if needed.
		2. easier to use common vocabulary to extend rules if needed.
	2. disadvantage: 
		1. will need some metaprogramming 

4. Coming up with operators and rules to write decision engine for circularity module
	1. advantage : 
		1. easier to use common vocabulary to write new rules if needed.
		2. easier to use common vocabulary to extend rules if needed.

	2. disadvantage: 
		1. will need extensive  metaprogramming  -- building interpreters and compilers.
	3. Prior art
		1. https://github.com/ympons/expreso
		2. using Tablex - decision table rules
		3. https://github.com/valiot/abacus
  

Scope 

  

1. Share projects = old and new 
2. Create a brand new repo 

1. Set of modules — logical circularity modules 
2. Define inputs and shape of them 
3. Pass the test. 
4. Context module / modules \

  

Communication channels — slow and fast. 

Looking at quality design and tests. Questions being asked. Decisions and tradeoffs being documented? 

  

TDD.