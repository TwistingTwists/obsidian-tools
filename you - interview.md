---
tags:
  - engineer
  - interview
  - revisit-often
libraries: 
topic: interview
---
-----

## Top level links to other files 

[[interview story]]
[[Talk about your experience]]


---

## System design 
https://github.com/shashank88/system_design?tab=readme-ov-file#designques

scalable sys design blogs -- https://github.com/binhnguyennus/awesome-scalability?tab=readme-ov-file#architecture

------


What happens when you type google.com in the browser
https://github.com/alex/what-happens-when

### Notes from taro 
https://www.youtube.com/watch?v=PJPFLHw8lKI

| topic                                                                                                                      | advice                                                                                                                                                        |
| -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <br><br>Code review <br>                                                                                                   | Delivering value MIGHT also be from copying. Software is moslty copy paste.<br><br>In new context, when you are new, find COPY                                |
| - tutorial book etc are guardrails - be creative. explore different ways ---- ask questions -- teach them to people        | Be at comfort with discomfort of your code not compiling in first go. That is the only way to go far.                                                         |
| reading code is harder than writing code <br><br>Reading code is debugging code<br><br>FIXER - go in codebase and fix that | Making change in a new system where you don't have context                                                                                                    |
| NETWORKING                                                                                                                 | Give more than you take with aim to HELP others SUCCEED.<br><br><br><br>engage with people to provide value to people and get help<br>Cultivate relationships |


| beliefs                                                         | instead, todo |
| --------------------------------------------------------------- | ------------- |
| negative politics + dark coporate place :p l               s  s |               |
|                                                                 |               |


|                |                                                                                                                                                                                                                                                              |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| GET interview  | - Your unfair advantage , What are you looking for?<br>- Resume = highlight reel of your professional accomplishment <br>- <br><br>Why UPSC, Why NOW, Why such a long break?<br><br>Why Google, Why now, Why <br><br><br>Get 10 interviews on the calendar?? |
| pass interview | - LEETCODE<br>- sys design                                                                                                                                                                                                                                   |



### Notes on Supabase

 [How we hire](https://supabase.com/blog/who-we-hire) document at supabse

|                                                                                                                     | evidence                                                                                                                                                                         | example of schelp                                                                                                                                            |
| ------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| people who take work off your plate rather than piling it on                                                        |                                                                                                                                                                                  |                                                                                                                                                              |
| effective in async work                                                                                             |                                                                                                                                                                                  |                                                                                                                                                              |
| [schelping](https://www.paulgraham.com/schlep.html)                                                                 | - just dive in                                                                                                                                                                   | Though the idea of fixing payments was right there in plain sight, they never saw it, because their unconscious mind shrank from the complications involved. |
| the founders grow with the problems.<br><br>List down the hardest problems you have solved in last year / last 6 mo |                                                                                                                                                                                  |                                                                                                                                                              |
| Solving Schelps                                                                                                     | Instead of asking "what problem should I solve?" ask "what problem do I wish someone else would solve for me?"                                                                   | e.g. Stripe                                                                                                                                                  |
| Why prefer Founders?                                                                                                | Something we especially value are team members who take on responsibility without asking and without being asked. Founders do this regularly.                                    |                                                                                                                                                              |
| Feature shipping                                                                                                    | 3 month product cycle - ambitious goal setting and achieving them.                                                                                                               |                                                                                                                                                              |
| Tech debt                                                                                                           | “Kaizen Weeks” that run within the first month of each 3 month cycle. The three weeks we ran as part of the last one were:<br><br>- QA week<br>- Docs week<br>- Performance week | An intense shipping schedule can lead to an accrual of technical debt.                                                                                       |
|                                                                                                                     |                                                                                                                                                                                  |                                                                                                                                                              |


#### Skills for Auth 

Supabase role 

- Strong knowledge of web technology fundamentals (cookies, sessions, JWT, HTTP, browser APIs)
    
- Good knowledge of and deep interest in authentication security (passwords, protocols such as OAuth, OIDC or SAML, cryptography fundamentals such as hash functions, signatures and ciphers)
    
- Experience working with multiple web frameworks like Next.js (or other SSR alternative in the JavaScript space) and traditional web frameworks like Ruby on Rails, Django, Laravel or equivalent (in any language)
    
- Strong knowledge of Go and TypeScript (languages used daily) and Postgres
    
- Good technical writing skills (RFC process is an important part of making changes to the Auth product)



FIRST ROUND 

Abhran 

- developing models of their own for voice and audio ??
- call centers - 
	- enterprises focus ; not on SME ; not on individuals 
	- sales team? 


generators vs iterators 
- efficiency -- which has less overhead -- generator vs iterator
- use case examples of them 


ContextManagers in python?

- leetcode coin changes questions 
	- infinite coins 
	- limited number of coins but find the nearest number - need to keep running sum 

-------------------- 

EM round Stord 
Sean Callan - EM 

- walk us through most impactful  project
	- core value - learning 
	- learnt - dif differently 
	- genstatemachine --- 
- user personas you have worked with?
	- 

What are different user personas you have worked with? 
- tinymesh 
- mathocart 
- australia team 


How did you know -- you were successful with the project?
- success metrics for your projects in past? 
	- FREELANCE
		- users don't talk about my product when I talk to them-- that's a baseline that it is not bad.
		- they mention that 'it works' - nothing fancy. just like flipkart :p 
	- JOB 
		- GenStateMachine -- packet drop rate -- down. more data transfer reliably
		- Caching in API -- request timeout
- Debugging issues on job?
	- stack - 
		- Grafana loki tempo 
		- aws cloudwatch 
	- examples 
		- postgres instance timeout for query >30s query processing time -- 1 billion record scan / aggregate -- (todo recheck the time) -- pagination for those queries
		- caching for most common point queries
	- 


At stord - engineer own code from dev to deployment -- supporting code and deployment

- troubleshooting / debugging how?
	- systemic bugs 
	- OTP bugs 
- postgres + rexbug + 
	- datadog + traces + 
- OpenTelemetry 



Worked with PM team before? 
- how involved in scoping?
- requirements are not abunduntaly clear 
- got the roadmpa -- important techincal` decision 
	- make sure ware + prioritised 
	- understand as a smaller group and then presenting to the PM 
- hard product / technical decision with 
	- liveview suggested at tinymesh
	- proactive + help people with skills (teaching elixir to js folks )


Do you have any questions for me? 
> Areas to ask question from -- product, tech, team, processes 

- product -- 
- tech 
- team 
- procceses


WMS + billing module (billing rules) + kafka  + 
- Luerl -- lua user validation code - robert virding


- WMS does CDC -> kafka 
	- external and internal event handlers 
	- INTEGRATION with PHYSICAL =  bluetooth scanners (barcode) + printers etc 
- OTP
	- GenServers + 
	- Stord github -- timeout tools for genserver 
		- 84% saving in processes -- sub second generation of genservers saving 
		- horde distributed nodes 
	- genserver - stateless

Phoenix Channels + postgres  + timescale DB (analytics of inventory) (inventory)

> doomspork! 
- printer guys 

Chattanoga conf -- billing team -- conference 


- WMS : PM overflow 
	- Lead time -- 
	- shipping packaging


inventory management + order management --- 
- schedlue folks to get the inventory distributed across 
- seconds matter -- beacuse 500 people save 1 secon => 500 seconds per day - 1 hr 



Questions from processes. 

Onboarding 
- 7 days first commit 
- help finds ticket to help get feet wet 
- 60+ days = contributing to velocity to project
- 90+ days = support call - 100% a contributor 


What interesting with stord ? 
- winter holiday -- shipping nad packaing - purchasing goes up => business goes up
- load testing -- pay off technical debt 
- reduce frequency of feature releases during peak 
- 

https://chat.openai.com/g/g-Xlb3igCde-feedback-artisan

 Feedback is the process of informing a person about behaviors that should be changed to improve a process.
 ![[58aba43b6e1efcdab336dde27ae8a4ded38f46f3e4ee30bff92b6d55fc3ae00e.png]]



1. Company switch -- why so frequent ?
	1. Tech stack - Elixir
	2. Would you be staying for longer period of time? 
		1. 
2. What do you bring to table? 
3. What are your expectations? 
	1. Salary competitive + appraisal cycles
	2. conducive environment regarding policies 
	3. Ask ?
4. Team handle
	1. Conflict of interest -- 
		1. two people on same appraisal cycle. 
	2. balance delivery schedule with chutti 
5. positively impacted a program or project you worked on
6. person you’ve worked with who’s had a great impact on you
7. most challenging problem you have faced and how did you solve it
8. disagreed with a team member


I am on a learning path. I am sure that I will seek guidance will be able to navigate through it.

Organisational Interest -- 
https://www.themartec.com/insidelook/behavioral-interview-questions
| Categories            | Questions |
| --------------------- | --------- |
| Problem Solving<br>   |           |
| Working on a Team<br> |           |
| Biggest Failures<br>  |           |
| Leadership<br>        |           |
| Personal Stress       |           |


#### Prepare 
1. Should I give feedback? 

| Roles   | _How might your role influence the feedback?_<br>    <br>    - _Are you providing feedback to a colleague?_<br>        <br>    - _As an EM, are you giving feedback to one of your team members?_<br>        <br>    - _As a developer, are you giving feedback to your EM?_                                                                                                    |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Context | Situation - location , observed behaviour<br><br>Events leading upto observed behaviour <br><br>- "What was going on when this behavior happened?"<br>- “Is there a clear pattern in this behavior?”<br>- "Could their background or personal life have something to do with it?"<br>- "How might my own perceptions or biases be influencing how I view the behavior?"<br><br> |
|         |                                                                                                                                                                                                                                                                                                                                                                                 |


#### Communicate

| Behaviour | Focus on the exact behaviour + use direct words to explain    |
| --------- | --- |
| Impact          |     |

### OKR meetings 
1. To make best tech company -- 3-5 year goal. What to do to aim towards this goal.
	1. Write tech blogs + contribute to OSS

## Team lead? 

1. Project lead? 
2. Problem Aaya / Solution 
	1. Timely - Daily sync up to remove blockers that they might have had.
	2. Gannt Chart - 
	3. 



## Disagreements 

I never liked the statement for work - "Let's agree to disagree".  
  
It could be an sign of failed communication.  
  
Maybe I dismissed someone else's perspective or didn't listen to them.  
  
To address this, I:  
  
- Ensure we understand and discuss the reasons for our disagreements.  
- Perhaps we weigh different intuitive factors differently. Maybe we are missing data.  
- Reflect on whether I am rushing the conversation to avoid a difficult discussion.  
- Let go if the topic is not crucial or the consequences are minor.  
- Try hard to never end the conversation on a sour note.


#### Round2 @remote.com 

#####################
- remote.com interview 

  

  

1. Intro 

5 years of experience + 

Broad experience highlight — hardware devices, RFC for encryption, MuonTraps (linux c groups) , frontend, background jobs, updating frontend, Tasks / supervisors / 

TCP and head of line blocking - real world consequences. 
  

Payroll vertical 

  

  

Why choose backend ? Over frontend? When you have significant frontend experience.

  

Why Remote? 

1. I respect the work that I have done in past. It pays my bills and get company business.
2. I am looking for is performance tuning and scale. 

  

  

Broad experience in general. Why remote then ? — scale brings me to remote
  

Proud project. mathocrat — front end, backend , RFC, 


Tiny mesh -team structure — 

- Team mates - there is an organisational hierarchy but not functional one. Anyone can connect with CTO as needed.
- CTO - Review work each week. 

  

  

  

  

  

  

Q. Product manager + management experiences 

  

Q. Done something that is not necessary but not direct responsibility.  But Improvement to product + teams operate + company operate 

  

Q. Decisions taken ? 0–0 make a call difficult decision  

- NMS design doc — stakeholders 
- On call rotation for dry run - at night also.

  

  

Stakeholders — 

Disagree with colleagues — discover new information.

- Colleagues + upper management + lower ? 
- You can always come closer if not on the middle ground. 

  

  

Code review — how to 

1. How many. 1, 2. More 
2. Initially, focus on finding examples. Knowledge gap + “reading more code” + reading docs
3. Then, when we make them sit in code review -> ask them what does this code do? — active engagement in code reviews.

  

  

  

How has been your experience at remote? 

- Elixir perspective. 

  

  

  

Shipping - feature flags 

  

payroll team + main api language. 

  

  

Next steps: 

  

1. Someone recruitment team — take home challenge, unlimited time

1. Make a tradeoff - either via readme.md 

3. Team interview - 3 engineers and review code challenges
#####################


### Remote.com 

#### Introduce yourself

I am a passionate software developer with more than 4 years of experience and with Elixir as primary language during 4 . 

I have had diverse experience using Elixir across backend, frontend and even LiveBook recently for internal dashboard tooling. 

During my last stint at Tinymesh, I have nurtured a team of juniors in elixir to help them gain experienced and be productive. We are more like a pro-sports team. I believe that giving feedback with care and effective communication are keys for a team to punch above their weight. 

I believe working with right people / teams makes a great work place not the free perks. Some of the best people who I lookup to, in Elixir Community have been at remote.com . That inspires me to join remote.com 


If you like, I can go on in more technical details on 

I have recently started contributing back to the elixir community when I can. I would love to give back more as time permits.

#### Why Remote.com ? 

First, friends referred me to this who have previously worked there. 

Second, I like the process from what I have gathered from the Remote Handbook and Interview Process. It is pretty detailed. 

Third, remote.


Questions: 
2. ProjectIAS went down in covid. How to verify ?


diversity and remote.com 
#### connections between your existing experience and the role you are interviewing for.

1. Tinymesh - LiveView + GenStateMachine + Broadway 
2. Mathocrat - as a freelance client -- march 2022  - (liveview 0.15 )
3. QuizDrill - telegram based quiz - heavy liveview usage

#### connections between our values, and you.

| Remote Values                      | Mine                            | Value Alignment                                          | related values                        |     |
| ---------------------------------- | ------------------------------- | -------------------------------------------------------- | ------------------------------------- | --- |
| Care = Kindness ++                 | communication + decision-making | Feedback = focus on improvement + appreciate achievement | responsible + accountable = ownershup |     |
|                                    |                                 | feedback = timely + intentional + with EQ                                                       |                                       |     |
|                                    |                                 | cont. improvement + feel seen n heard                    | trust in team                         |     |
| Care = how you deliver the message |                                 | don't disregard their time, effort and contribution      |                                       |     |
|                                    |                                 |                                                          |                                       |     |


apply 
call 
and be in thr proces 


#### What you are looking for? 


### 3 stage process
#### General tips 
1. pause after answer 
2. stick to question 
3. show motivation 
4. 
### Advise 

1. do something that you don't think you can do.

> When you practice music, it shouldn’t sound good. If you always sound
good during practice sessions, it means you’re not stretching your lim-
its. That’s what practice is for.

2. Stand on shoulder of giants

>  Sometimes we would play name
that improviser where one of us would play a recording of an impro-
vised solo and the rest of us would have to figure out, based on style,
who the recorded improviser was.

3. 


## Learnings from Project IAS 

1. Small measurable progress - 10% every week. consistency.
2. Customer feedback is important 
3. Monitoring in production - you must know the bug before your customer does.
4. Marketing and sales has a huge role to play in business.
	1. even though this part of job is boring, it is a skill and can't be built overnight.
5. 

### Senior Engineer (Manager) - Domain Knowledge

* NMS project  - stakeholder approach 
* Manage teams - with subordinates and upper management  - process and tools - 
* Domain 
	* Embedded : Launchboard + FTDI module + C language firmware + alpine + RF module (design + fabrication both in India + testing in our own lab)
	* IoT :  MQTT brokers (vernemq)
	* RF communication : mesh formation  ( 865 - 867 MHz, 868 also trying to include since this is unlicensed band {free ISM band} ) + Wireshark etc for debugging
	* Meter : DLMS protocol (IS 15959) + OBIS codes (400+ worked on)

## Gaps to fill
1. How to design a good HTTP Client ? - Hardhat + Req + telemetry + fuse?
2. Learn Ecto from firezone github project 
3. Supavisor - commit by commit 
4. [Binaries and Elixir](binary_pattern_matching) 
5. [Concurrency in Elixir](concurrent_elixir) 


### Mentoring yourself

Here’s a way to proxy-mentor yourself.

Think of the person in your field whom you admire most. Most
of us have a short list already formulated from some stage in
our careers. It may be someone we’ve worked with, or it may
be someone whose work we admire. List the ten most important
attributes of this role model. Choose the attributes that are the
reason why you have chosen this person to be your role model.
These attributes might be specific areas of skill, such as technol-
ogy breadth, or the depth of their knowledge in some particular
domain. Or, they might be more personal traits like the ability to
make team members comfortable or that they are an engaging
speaker.
Now, rank those qualities in order of importance, with 1 being the
least important and 10 being the most important. You have now
created and distilled a list of attributes that you find admirable
and important. These are the ways in which you should strive to
emulate your chosen role model. But, how do you choose which
to focus on first?
Add a column to the list, and for each item on the list, imagine how
your role model would rate you on a scale of 1 to 10 (10 being the
best). Try to really put yourself into the mind of your role model and
to observe yourself as if a third person.
When you have the attributes, ranking, and your own ratings, in a
final column subtract your rating in each row from the importance
level you gave it in the preceding column. If you ranked something
as 10, the most important attribute of your role model, and your
rating was 3, that gives you a final priority score of 7. Having filled
this column in completely, sorting in descending order will you give
a prioritized top ten list of areas in which you need to improve.
Start with the top two or three items, and put together a concrete
list of tasks you can start doing now to improve yourself


### How to view a company?
A lot of companies think of themselves like a family. We are not a family. *We are like a pro sports team*. Families mean unconditional love. Once born in to the family you are part of it forever. A great team is about pushing yourself and your teammates, caring intensively about the team. It is about performance and not seniority or how long you have been with the team. Yet unlike in a family players know they will not play for the team forever. A team can only be as good as its weakest member. That’s why a great coach will always try to have the best players on every position.

## How to showcase your work? 

- [ ] 


### Books on marketing 
- [ ] Good to Great 
- [ ] This is Marketing - seth godin 
- [ ] 

## What is your life story? 


### Why leave this job? 

### Leadership quality in past


### Directly from JD 

DAILY ADVENTURES/RESPONSIBILITIES

  Participation in key technical decision-making discussions such as sprint planning, software design, and code reviews
  Help build and scale a world-class engineering team by interviewing and referral.
  Hold systematic weekly 1:1 touchpoints with engineers to deliver and receive quality feedback
  Setting technical direction and overseeing engineering projects working along with CTO on deciding the technology roadmap.
  Mentorship and sponsorship - best way to grow those around you is by creating an active practice of mentorship and sponsorship. Maintaining relationships and developing a positive team culture
  Provide objective and helpful quarterly performance feedback for engineers


Examples:
1. Code reviews - what to focus on?
2. Create culture? - start by saying right things. lead by example.



#### Scenario

- How will you deploy customer application on-prem?? 
- 



https://aistudio.google.com/prompts/1aN4vtjb4R3R6GLnJWwGINk1bvkOr530E



| Scenario                                                                                                            | Radical Candor                                                                                                                                                                                                                                                                                                                                                                                                             | How it shows empathy?                                                                                                                                        | Alternate Statement or Course of Action for Scenario                                                                                                                                                                                                                                                                | Follow Ups                                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| A team member consistently submits work with minor but noticeable errors.                                           | "Hey [Team Member Name], I've noticed a pattern of small errors in your recent submissions, like the typo on slide 3 and the incorrect data point on page 5 of the report. This impacts the overall credibility of our work. I know you're capable of producing high-quality work, and I want to help you ensure these details are accurate. What support do you need to catch these before submitting?"                   | Focuses on the impact on the team and the quality of work, expressing belief in their ability and offering support. It's not accusatory but problem-focused. | **Ruinous Empathy:** Saying nothing to avoid hurting their feelings. **Obnoxious Aggression:** "Your work is sloppy and unprofessional!" **Manipulative Insincerity:** "Great job! Just a few tiny things to tweak." (without specifying)                                                                           | Offer specific tools or training. Schedule a brief check-in before submissions for a while. Acknowledge improvement when seen.                                     |
| An employee interrupts others frequently in meetings.                                                               | "[Employee Name], I've noticed you often jump in before others have finished speaking in meetings. While your enthusiasm is great, it can make others feel unheard and disrupt the flow. It's important everyone feels comfortable sharing their thoughts. Could you try to let others finish before jumping in?"                                                                                                          | Connects the behavior to the impact on others and the meeting environment. Focuses on the desired outcome (everyone feeling heard).                          | **Ruinous Empathy:** Saying nothing and letting the behavior continue. **Obnoxious Aggression:** "Stop interrupting everyone!" **Manipulative Insincerity:**  Joking about it privately with others instead of addressing it directly.                                                                              | Gently redirect the conversation if it happens again. Privately acknowledge when they've improved. Set meeting ground rules for respectful communication.          |
| A colleague presents an idea that is flawed and won't work.                                                         | "That's an interesting angle, [Colleague Name]. I'm seeing a potential challenge with [specific aspect of the idea] because [explain the reasoning/potential roadblock]. Have you considered [alternative approach or question to prompt them to think further]? I'm happy to brainstorm solutions with you."                                                                                                              | Acknowledges their contribution while directly addressing the flaw with concrete reasons. Offers collaboration and support.                                  | **Ruinous Empathy:**  Praising the idea despite its flaws to avoid conflict. **Obnoxious Aggression:** "That's a terrible idea! It will never work." **Manipulative Insincerity:**  Agreeing in the meeting and then undermining the idea later.                                                                    | Be open to their response and willing to discuss further. If the alternative approach is pursued, offer support.                                                   |
| A team member is consistently late for team meetings.                                                               | "[Team Member Name], I've noticed you've been late to the last few team meetings. This makes it difficult to start on time and can be disruptive for others who are prompt. Is there anything preventing you from arriving on time that we can help with?"                                                                                                                                                                 | Directly addresses the pattern of behavior and the impact on the team. Opens the door for understanding and offering support if there's a legitimate reason. | **Ruinous Empathy:** Ignoring the lateness to avoid confrontation. **Obnoxious Aggression:** "You're always late! Get it together!" **Manipulative Insincerity:**  Making passive-aggressive comments about punctuality without directly addressing the individual.                                                 | If a reason is given, explore solutions. If not, reiterate the importance of punctuality and its impact.                                                           |
| An employee seems disengaged and isn't contributing much in discussions.                                            | "Hey [Employee Name], I've noticed you've been a bit quieter in our recent discussions. I value your perspective and want to make sure you feel comfortable sharing your thoughts. Is there anything that's preventing you from contributing more, or is there anything we can do to make the discussions more engaging for you?"                                                                                          | Shows they are noticed and valued. Expresses a desire to hear their input and seeks to understand the reason for disengagement.                              | **Ruinous Empathy:**  Assuming they are just introverted and not addressing it. **Obnoxious Aggression:** "Why aren't you saying anything? You're not contributing!" **Manipulative Insincerity:**  Asking leading questions designed to get the answer they want instead of genuinely listening.                   | Create space for quieter voices in meetings. Follow up individually to understand their preferences for communication.                                             |
| A direct report is consistently missing deadlines.                                                                  | "[Direct Report Name], I've noticed that the last few deadlines haven't been met. This is impacting our project timeline and the work of others. Let's talk about what's causing these delays. Are there any roadblocks I can help remove, or do we need to adjust the workload or approach?"                                                                                                                              | Focuses on the impact and seeks to understand the underlying cause rather than just assigning blame. Offers support and explores potential solutions.        | **Ruinous Empathy:** Making excuses for them and not addressing the issue. **Obnoxious Aggression:** "You're always missing deadlines! You're unreliable!" **Manipulative Insincerity:**  Making vague threats about performance without addressing the specific issues.                                            | Work together to create a plan to meet future deadlines. Provide necessary resources or training. Monitor progress and offer ongoing support.                      |
| A colleague is taking credit for your ideas.                                                                        | "[Colleague Name], I noticed during the presentation you mentioned [your idea] as your own. I understand that sometimes things get misattributed, but it's important to me that credit is given where it's due. In the future, could we agree on how to properly attribute contributions?"                                                                                                                                 | Addresses the specific behavior and explains the impact on you directly and calmly. Focuses on a solution for future interactions.                           | **Ruinous Empathy:** Saying nothing to avoid conflict. **Obnoxious Aggression:** "You stole my idea!" **Manipulative Insincerity:**  Complaining to others behind their back instead of addressing it directly.                                                                                                     | Observe future interactions and address any further instances promptly. Consider discussing team norms around idea attribution.                                    |
| An employee is exhibiting a negative attitude that is impacting team morale.                                        | "[Employee Name], I've noticed you've seemed a bit down lately, and some of your comments in team meetings have been quite negative. I'm concerned about the impact this has on the team's morale. Is everything okay? Is there anything I can do to help, or anything you'd like to talk about?"                                                                                                                          | Expresses concern for their well-being and acknowledges the impact on the team. Opens the door for a personal conversation and offers support.               | **Ruinous Empathy:** Ignoring the negativity to avoid an uncomfortable conversation. **Obnoxious Aggression:** "Stop being so negative! You're bringing everyone down." **Manipulative Insincerity:**  Trying to cheer them up with forced positivity without addressing the underlying issue.                      | Listen empathetically to their concerns. If appropriate, offer resources or support. If the negativity continues, address the specific behaviors and their impact. |
| A new team member is struggling with a specific task.                                                               | "[New Team Member Name], I've noticed you've been working on [task] for a while, and I just wanted to check in and see how it's going. Is there anything I can clarify or any resources that would be helpful? I remember when I first started, [similar experience], so don't hesitate to ask for support."                                                                                                               | Acknowledges their effort and offers specific help. Shares a relatable personal experience to normalize seeking support.                                     | **Ruinous Empathy:** Assuming they will figure it out on their own without offering support. **Obnoxious Aggression:** "Why haven't you finished this yet? It's not that hard!" **Manipulative Insincerity:**  Offering vague help that isn't actually useful.                                                      | Provide specific guidance, resources, or mentorship. Check in regularly to offer ongoing support and encouragement.                                                |
| A colleague consistently uses jargon that makes it difficult for others to understand.                              | "[Colleague Name], you have a lot of expertise in [area], and I really value your input. Sometimes when you're explaining things, you use terms like [mention specific jargon]. While I understand what you mean, I've noticed others in the meeting seem a bit lost. Could you maybe explain those concepts in simpler terms, especially when we're talking to a broader audience?"                                       | Acknowledges their expertise while directly addressing the communication issue and its impact on others. Offers a specific suggestion for improvement.       | **Ruinous Empathy:** Pretending to understand and then being confused later. **Obnoxious Aggression:** "Stop using all that jargon! No one knows what you're talking about!" **Manipulative Insincerity:**  Making jokes about the jargon without directly addressing the colleague.                                | Model clear and simple communication yourself. Gently interject and ask for clarification during meetings if needed.                                               |
| Team member submits late work                                                                                       | "I see the work is late, and I know you're capable of managing deadlines. Is there anything blocking you?"                                                                                                                                                                                                                                                                                                                 | Acknowledges their potential and offers support                                                                                                              | "This delay is unacceptable. Fix it."                                                                                                                                                                                                                                                                               | Discuss their workload or obstacles to improve next time.                                                                                                          |
| Colleague misses a key meeting                                                                                      | "I noticed you missed the meeting. Is everything okay? Let’s plan how to keep you in the loop."                                                                                                                                                                                                                                                                                                                            | Assumes good intent and cares about their well-being                                                                                                         | "You missed the meeting again. This can't happen."                                                                                                                                                                                                                                                                  | Share meeting notes and ensure clarity for future plans.                                                                                                           |
| Employee delivers a confusing report                                                                                | "The report could be clearer in these areas. You’re great at structuring ideas; let’s refine this."                                                                                                                                                                                                                                                                                                                        | Highlights their strengths and offers constructive help                                                                                                      | "This report is confusing and poorly done."                                                                                                                                                                                                                                                                         | Review the revised report and provide further feedback.                                                                                                            |
| Peer dominates team discussions                                                                                     | "You’re enthusiastic, but others need a chance to share too. Let’s work on balancing input."                                                                                                                                                                                                                                                                                                                               | Values their passion but highlights the need for balance                                                                                                     | "You need to stop talking so much in meetings."                                                                                                                                                                                                                                                                     | Observe future meetings and provide encouragement.                                                                                                                 |
| Team member seems disengaged                                                                                        | "You seem less engaged lately. Is there something on your mind? How can I help?"                                                                                                                                                                                                                                                                                                                                           | Shows concern for their emotional state                                                                                                                      | "Why aren’t you putting in effort lately?"                                                                                                                                                                                                                                                                          | Schedule regular check-ins to maintain engagement.                                                                                                                 |
| Colleague interrupts in meetings                                                                                    | "I noticed you interrupt often, which might discourage others. Let’s give everyone space to speak."                                                                                                                                                                                                                                                                                                                        | Addresses the issue while valuing their input                                                                                                                | "Stop interrupting everyone; it’s disruptive."                                                                                                                                                                                                                                                                      | Monitor discussions and praise balanced participation.                                                                                                             |
| Frequent errors in a colleague's work                                                                               | "I see recurring errors in your work. Let’s identify causes and work on solutions together."                                                                                                                                                                                                                                                                                                                               | Focuses on solving the problem collaboratively                                                                                                               | "You’re making too many mistakes. This isn’t acceptable."                                                                                                                                                                                                                                                           | Review work quality after implementing solutions.                                                                                                                  |
| Team member avoids taking responsibility                                                                            | "I noticed you deflected responsibility. I know you can take ownership. Let’s address this together."                                                                                                                                                                                                                                                                                                                      | Expresses belief in their ability to improve                                                                                                                 | "You always avoid accountability. Take responsibility."                                                                                                                                                                                                                                                             | Follow up to see improved ownership in future tasks.                                                                                                               |
| Peer delivers exceptional work                                                                                      | "This work is excellent! Your attention to detail really shows. Great job!"                                                                                                                                                                                                                                                                                                                                                | Recognizes and appreciates their efforts                                                                                                                     | "Good work."                                                                                                                                                                                                                                                                                                        | Continue providing positive feedback when they excel.                                                                                                              |
| Subordinate struggles with presentations                                                                            | "Your presentation lacked clarity, but I’ve seen you communicate well before. Let’s work on this."                                                                                                                                                                                                                                                                                                                         | Reassures them of their abilities while addressing gaps                                                                                                      | "That presentation wasn’t good at all. Fix it next time."                                                                                                                                                                                                                                                           | Help them rehearse or offer tips for the next presentation.                                                                                                        |
| A team member consistently misses key information shared in meetings and acts on outdated info.                     | "[Team Member Name], I've noticed a few times now you've seemed to miss crucial information shared in meetings, like the change in the project deadline we discussed on Monday. This has led to some rework. I know it can be a lot to keep track of, is there anything we can do to make sure you have the right information, like better note-taking or recaps?"                                                         | Focuses on the impact of the missed information (rework) rather than blaming the individual. Offers support and solutions.                                   | **Ruinous Empathy:**  Re-explaining the information every time without addressing the underlying issue. **Obnoxious Aggression:** "You never pay attention! Why are you always working with the wrong info?" **Manipulative Insincerity:**  Silently correcting their mistakes without ever addressing the pattern. | Offer to pair them with someone for meeting notes. Explore different communication channels for key updates.                                                       |
| An employee expresses resistance to a new process or tool being implemented.                                        | "[Employee Name], I understand you're feeling hesitant about using the new CRM system. You mentioned you prefer the old method because of [specific reason]. I hear that, and change can be tough. However, this new system will ultimately help us [explain benefits – e.g., streamline workflows, improve data accuracy]. Let's talk about your specific concerns and how we can make this transition smoother for you." | Acknowledges their feelings and validates their perspective. Explains the rationale behind the change and offers to address their specific concerns.         | **Ruinous Empathy:** Abandoning the new process because one person is resistant. **Obnoxious Aggression:** "Just do it! It's the new way, deal with it." **Manipulative Insincerity:**  Telling them it's their choice but then making it clear they are expected to use the new system.                            | Offer training and resources. Create a forum for feedback and address concerns. Highlight early successes with the new system.                                     |
| A colleague has a persistent habit that annoys others (e.g., loud phone calls in a shared space).                   | "[Colleague Name],  I wanted to chat about something that's been a bit disruptive in our shared workspace. When you're on phone calls, especially the louder ones, it makes it difficult for others to concentrate. I understand you need to take calls, and perhaps we could explore a solution together, like using a meeting room or lowering your voice slightly?"                                                     | Focuses on the impact on others and the shared workspace. Offers collaborative problem-solving.                                                              | **Ruinous Empathy:**  Suffering in silence or complaining to others without addressing the colleague. **Obnoxious Aggression:** "Can you shut up on your calls? You're so loud!" **Manipulative Insincerity:**  Making sarcastic comments about their volume.                                                       | Suggest headphones for personal calls. Establish shared workspace etiquette.                                                                                       |
| A direct report consistently focuses on problems without offering solutions.                                        | "[Direct Report Name], I appreciate you bringing these challenges to my attention. While identifying problems is important, I'd also like to encourage you to think about potential solutions. Next time you bring up an issue, could you also come prepared with a couple of ideas on how we might address it?  This will help us move forward more effectively."                                                         | Acknowledges the value of identifying problems but gently redirects towards proactive problem-solving.                                                       | **Ruinous Empathy:**  Always jumping in to solve the problems for them. **Obnoxious Aggression:** "Stop complaining and just fix it!" **Manipulative Insincerity:**  Agreeing with their complaints but then doing nothing about them.                                                                              | Offer training on problem-solving methodologies. Praise and acknowledge when they offer solutions.                                                                 |
| A colleague isn't pulling their weight on a team project.                                                           | "[Colleague Name], let's touch base on the progress of the project. I've noticed that [specific task] hasn't been completed yet, and we're approaching the deadline. Is there anything I can do to support you in getting that done?  We need everyone contributing to ensure we meet our goals."                                                                                                                          | Focuses on the specific task and the impact on the project and team goals. Offers support rather than accusation.                                            | **Ruinous Empathy:**  Picking up their slack to avoid conflict or project delays. **Obnoxious Aggression:** "You're not doing anything! You're going to make us miss the deadline!" **Manipulative Insincerity:**  Complaining about their lack of contribution to other team members.                              | Clarify roles and responsibilities. Offer assistance or resources. Escalate to a manager if the issue persists.                                                    |
| An employee is consistently unprepared for meetings.                                                                | "[Employee Name], I've noticed in the last few meetings you haven't seemed to have reviewed the pre-reading materials or are unfamiliar with the agenda topics. This makes it harder for us to have productive discussions. Is there anything preventing you from preparing beforehand, or is there anything we can do to make the preparation process easier?"                                                            | Directly addresses the behavior and its impact on meeting productivity. Seeks to understand the reason and offer support.                                    | **Ruinous Empathy:**  Summarizing everything in detail for them in the meeting. **Obnoxious Aggression:** "Why are you so unprepared? You're wasting everyone's time!" **Manipulative Insincerity:**  Assigning them tasks they can't possibly complete without the meeting context.                                | Clearly communicate expectations for meeting preparation. Provide materials well in advance. Consider brief pre-reads together as a team.                          |
| A team member has a habit of taking over conversations and dominating discussions.                                  | "[Team Member Name], you have valuable insights and I appreciate your contributions to our discussions. I've also noticed that sometimes it can be difficult for others to get a word in. Let's be mindful of creating space for everyone to share their perspectives so we can benefit from the full range of ideas in the team."                                                                                         | Acknowledges their positive contributions while directly addressing the negative behavior and its impact on inclusivity.                                     | **Ruinous Empathy:**  Saying nothing and letting them dominate, making others feel unheard. **Obnoxious Aggression:** "Stop talking! You're always talking!" **Manipulative Insincerity:**  Trying to subtly interrupt them without directly addressing the issue.                                                  | Use structured meeting formats that ensure equal participation. Gently redirect conversations if needed.                                                           |
| A colleague consistently delivers work that is "good enough" but not their best effort.                             | "[Colleague Name], the work you submitted on the [project] is functional, and it meets the basic requirements. However, I know you're capable of producing higher quality work, and I've seen you do it before. Is there anything that's preventing you from putting in that extra level of detail or polish?  I want to see you succeed and produce your best work."                                                      | Acknowledges the minimum standard while expressing belief in their potential and a desire to see them excel.                                                 | **Ruinous Empathy:**  Praising mediocre work to avoid hurting their feelings. **Obnoxious Aggression:** "This is lazy! You didn't even try!" **Manipulative Insincerity:**  Giving vague feedback like "it's okay" without pointing out areas for improvement.                                                      | Provide specific examples of past high-quality work. Offer opportunities for skill development or mentorship.                                                      |
| An employee is resistant to feedback and becomes defensive when receiving constructive criticism.                   | "[Employee Name], I've noticed that when I offer suggestions for improvement, you sometimes seem to get a bit defensive. My intention is always to help you grow and develop your skills. It's important for us to be able to have open and honest conversations about how we can all improve. Is there a way I can frame feedback that would be more helpful for you?"                                                    | Focuses on their intention and the importance of feedback for growth. Opens a dialogue about how to deliver feedback more effectively for that individual.   | **Ruinous Empathy:**  Avoiding giving any feedback to prevent discomfort. **Obnoxious Aggression:** "You can't take criticism! You're too sensitive!" **Manipulative Insincerity:**  Giving positive feedback while secretly criticizing their performance to others.                                               | Schedule regular feedback sessions and create a safe space for discussion. Focus on specific behaviors and their impact.                                           |
| A team member frequently complains about workload but doesn't proactively seek solutions or prioritize effectively. | "[Team Member Name], I hear you're feeling overwhelmed with your workload. It sounds stressful. Let's take a look at your current tasks and see if we can prioritize them more effectively, delegate some if possible, or identify any bottlenecks. Have you considered [specific time management technique]?"                                                                                                             | Acknowledges their feelings and validates their experience. Shifts the focus to proactive solutions and offers specific strategies.                          | **Ruinous Empathy:**  Taking on their work to alleviate their stress. **Obnoxious Aggression:** "Everyone is busy! Stop complaining and just get it done!" **Manipulative Insincerity:**  Agreeing they have too much work but then assigning them even more.                                                       | Provide training on time management and prioritization. Regularly review workloads and offer support in planning.                                                  |


----


| Category of Scenario              | Scenario                                                                                                            | Radical Candor                                                                                                                                                                                                                                                                                                                                                                                                             | How it shows empathy?                                                                                                                                        | Alternate Statement or Course of Action for Scenario                                                                                                                                                                                                                                                                | Follow Ups                                                                                                                                                         |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Work Quality**                  | A team member consistently submits work with minor but noticeable errors.                                           | "Hey [Team Member Name], I've noticed a pattern of small errors in your recent submissions, like the typo on slide 3 and the incorrect data point on page 5 of the report. This impacts the overall credibility of our work. I know you're capable of producing high-quality work, and I want to help you ensure these details are accurate. What support do you need to catch these before submitting?"                   | Focuses on the impact on the team and the quality of work, expressing belief in their ability and offering support. It's not accusatory but problem-focused. | **Ruinous Empathy:** Saying nothing to avoid hurting their feelings. **Obnoxious Aggression:** "Your work is sloppy and unprofessional!" **Manipulative Insincerity:** "Great job! Just a few tiny things to tweak." (without specifying)                                                                           | Offer specific tools or training. Schedule a brief check-in before submissions for a while. Acknowledge improvement when seen.                                     |
| **Meeting Behavior**              | An employee interrupts others frequently in meetings.                                                               | "[Employee Name], I've noticed you often jump in before others have finished speaking in meetings. While your enthusiasm is great, it can make others feel unheard and disrupt the flow. It's important everyone feels comfortable sharing their thoughts. Could you try to let others finish before jumping in?"                                                                                                          | Connects the behavior to the impact on others and the meeting environment. Focuses on the desired outcome (everyone feeling heard).                          | **Ruinous Empathy:** Saying nothing and letting the behavior continue. **Obnoxious Aggression:** "Stop interrupting everyone!" **Manipulative Insincerity:**  Joking about it privately with others instead of addressing it directly.                                                                              | Gently redirect the conversation if it happens again. Privately acknowledge when they've improved. Set meeting ground rules for respectful communication.          |
| **Idea/Contribution**             | A colleague presents an idea that is flawed and won't work.                                                         | "That's an interesting angle, [Colleague Name]. I'm seeing a potential challenge with [specific aspect of the idea] because [explain the reasoning/potential roadblock]. Have you considered [alternative approach or question to prompt them to think further]? I'm happy to brainstorm solutions with you."                                                                                                              | Acknowledges their contribution while directly addressing the flaw with concrete reasons. Offers collaboration and support.                                  | **Ruinous Empathy:**  Praising the idea despite its flaws to avoid conflict. **Obnoxious Aggression:** "That's a terrible idea! It will never work." **Manipulative Insincerity:**  Agreeing in the meeting and then undermining the idea later.                                                                    | Be open to their response and willing to discuss further. If the alternative approach is pursued, offer support.                                                   |
| **Time Management**               | A team member is consistently late for team meetings.                                                               | "[Team Member Name], I've noticed you've been late to the last few team meetings. This makes it difficult to start on time and can be disruptive for others who are prompt. Is there anything preventing you from arriving on time that we can help with?"                                                                                                                                                                 | Directly addresses the pattern of behavior and the impact on the team. Opens the door for understanding and offering support if there's a legitimate reason. | **Ruinous Empathy:** Ignoring the lateness to avoid confrontation. **Obnoxious Aggression:** "You're always late! Get it together!" **Manipulative Insincerity:**  Making passive-aggressive comments about punctuality without directly addressing the individual.                                                 | If a reason is given, explore solutions. If not, reiterate the importance of punctuality and its impact.                                                           |
| **Engagement/Contribution**       | An employee seems disengaged and isn't contributing much in discussions.                                            | "Hey [Employee Name], I've noticed you've been a bit quieter in our recent discussions. I value your perspective and want to make sure you feel comfortable sharing your thoughts. Is there anything that's preventing you from contributing more, or is there anything we can do to make the discussions more engaging for you?"                                                                                          | Shows they are noticed and valued. Expresses a desire to hear their input and seeks to understand the reason for disengagement.                              | **Ruinous Empathy:**  Assuming they are just introverted and not addressing it. **Obnoxious Aggression:** "Why aren't you saying anything? You're not contributing!" **Manipulative Insincerity:**  Asking leading questions designed to get the answer they want instead of genuinely listening.                   | Create space for quieter voices in meetings. Follow up individually to understand their preferences for communication.                                             |
| **Time Management/Performance**   | A direct report is consistently missing deadlines.                                                                  | "[Direct Report Name], I've noticed that the last few deadlines haven't been met. This is impacting our project timeline and the work of others. Let's talk about what's causing these delays. Are there any roadblocks I can help remove, or do we need to adjust the workload or approach?"                                                                                                                              | Focuses on the impact and seeks to understand the underlying cause rather than just assigning blame. Offers support and explores potential solutions.        | **Ruinous Empathy:** Making excuses for them and not addressing the issue. **Obnoxious Aggression:** "You're always missing deadlines! You're unreliable!" **Manipulative Insincerity:**  Making vague threats about performance without addressing the specific issues.                                            | Work together to create a plan to meet future deadlines. Provide necessary resources or training. Monitor progress and offer ongoing support.                      |
| **Collaboration/Recognition**     | A colleague is taking credit for your ideas.                                                                        | "[Colleague Name], I noticed during the presentation you mentioned [your idea] as your own. I understand that sometimes things get misattributed, but it's important to me that credit is given where it's due. In the future, could we agree on how to properly attribute contributions?"                                                                                                                                 | Addresses the specific behavior and explains the impact on you directly and calmly. Focuses on a solution for future interactions.                           | **Ruinous Empathy:** Saying nothing to avoid conflict. **Obnoxious Aggression:** "You stole my idea!" **Manipulative Insincerity:**  Complaining to others behind their back instead of addressing it directly.                                                                                                     | Observe future interactions and address any further instances promptly. Consider discussing team norms around idea attribution.                                    |
| **Attitude/Morale**               | An employee is exhibiting a negative attitude that is impacting team morale.                                        | "[Employee Name], I've noticed you've seemed a bit down lately, and some of your comments in team meetings have been quite negative. I'm concerned about the impact this has on the team's morale. Is everything okay? Is there anything I can do to help, or anything you'd like to talk about?"                                                                                                                          | Expresses concern for their well-being and acknowledges the impact on the team. Opens the door for a personal conversation and offers support.               | **Ruinous Empathy:** Ignoring the negativity to avoid an uncomfortable conversation. **Obnoxious Aggression:** "Stop being so negative! You're bringing everyone down." **Manipulative Insincerity:**  Trying to cheer them up with forced positivity without addressing the underlying issue.                      | Listen empathetically to their concerns. If appropriate, offer resources or support. If the negativity continues, address the specific behaviors and their impact. |
| **Onboarding/Skill Development**  | A new team member is struggling with a specific task.                                                               | "[New Team Member Name], I've noticed you've been working on [task] for a while, and I just wanted to check in and see how it's going. Is there anything I can clarify or any resources that would be helpful? I remember when I first started, [similar experience], so don't hesitate to ask for support."                                                                                                               | Acknowledges their effort and offers specific help. Shares a relatable personal experience to normalize seeking support.                                     | **Ruinous Empathy:** Assuming they will figure it out on their own without offering support. **Obnoxious Aggression:** "Why haven't you finished this yet? It's not that hard!" **Manipulative Insincerity:**  Offering vague help that isn't actually useful.                                                      | Provide specific guidance, resources, or mentorship. Check in regularly to offer ongoing support and encouragement.                                                |
| **Communication**                 | A colleague consistently uses jargon that makes it difficult for others to understand.                              | "[Colleague Name], you have a lot of expertise in [area], and I really value your input. Sometimes when you're explaining things, you use terms like [mention specific jargon]. While I understand what you mean, I've noticed others in the meeting seem a bit lost. Could you maybe explain those concepts in simpler terms, especially when we're talking to a broader audience?"                                       | Acknowledges their expertise while directly addressing the communication issue and its impact on others. Offers a specific suggestion for improvement.       | **Ruinous Empathy:** Pretending to understand and then being confused later. **Obnoxious Aggression:** "Stop using all that jargon! No one knows what you're talking about!" **Manipulative Insincerity:**  Making jokes about the jargon without directly addressing the colleague.                                | Model clear and simple communication yourself. Gently interject and ask for clarification during meetings if needed.                                               |
| **Work Quality**                  | Team member submits late work                                                                                       | "I see the work is late, and I know you're capable of managing deadlines. Is there anything blocking you?"                                                                                                                                                                                                                                                                                                                 | Acknowledges their potential and offers support                                                                                                              | "This delay is unacceptable. Fix it."                                                                                                                                                                                                                                                                               | Discuss their workload or obstacles to improve next time.                                                                                                          |
| **Meeting Behavior**              | Colleague misses a key meeting                                                                                      | "I noticed you missed the meeting. Is everything okay? Let’s plan how to keep you in the loop."                                                                                                                                                                                                                                                                                                                            | Assumes good intent and cares about their well-being                                                                                                         | "You missed the meeting again. This can't happen."                                                                                                                                                                                                                                                                  | Share meeting notes and ensure clarity for future plans.                                                                                                           |
| **Work Quality**                  | Employee delivers a confusing report                                                                                | "The report could be clearer in these areas. You’re great at structuring ideas; let’s refine this."                                                                                                                                                                                                                                                                                                                        | Highlights their strengths and offers constructive help                                                                                                      | "This report is confusing and poorly done."                                                                                                                                                                                                                                                                         | Review the revised report and provide further feedback.                                                                                                            |
| **Meeting Behavior**              | Peer dominates team discussions                                                                                     | "You’re enthusiastic, but others need a chance to share too. Let’s work on balancing input."                                                                                                                                                                                                                                                                                                                               | Values their passion but highlights the need for balance                                                                                                     | "You need to stop talking so much in meetings."                                                                                                                                                                                                                                                                     | Observe future meetings and provide encouragement.                                                                                                                 |
| **Engagement/Well-being**         | Team member seems disengaged                                                                                        | "You seem less engaged lately. Is there something on your mind? How can I help?"                                                                                                                                                                                                                                                                                                                                           | Shows concern for their emotional state                                                                                                                      | "Why aren’t you putting in effort lately?"                                                                                                                                                                                                                                                                          | Schedule regular check-ins to maintain engagement.                                                                                                                 |
| **Meeting Behavior**              | Colleague interrupts in meetings                                                                                    | "I noticed you interrupt often, which might discourage others. Let’s give everyone space to speak."                                                                                                                                                                                                                                                                                                                        | Addresses the issue while valuing their input                                                                                                                | "Stop interrupting everyone; it’s disruptive."                                                                                                                                                                                                                                                                      | Monitor discussions and praise balanced participation.                                                                                                             |
| **Work Quality**                  | Frequent errors in a colleague's work                                                                               | "I see recurring errors in your work. Let’s identify causes and work on solutions together."                                                                                                                                                                                                                                                                                                                               | Focuses on solving the problem collaboratively                                                                                                               | "You’re making too many mistakes. This isn’t acceptable."                                                                                                                                                                                                                                                           | Review work quality after implementing solutions.                                                                                                                  |
| **Accountability**                | Team member avoids taking responsibility                                                                            | "I noticed you deflected responsibility. I know you can take ownership. Let’s address this together."                                                                                                                                                                                                                                                                                                                      | Expresses belief in their ability to improve                                                                                                                 | "You always avoid accountability. Take responsibility."                                                                                                                                                                                                                                                             | Follow up to see improved ownership in future tasks.                                                                                                               |
| **Positive Feedback**             | Peer delivers exceptional work                                                                                      | "This work is excellent! Your attention to detail really shows. Great job!"                                                                                                                                                                                                                                                                                                                                                | Recognizes and appreciates their efforts                                                                                                                     | "Good work."                                                                                                                                                                                                                                                                                                        | Continue providing positive feedback when they excel.                                                                                                              |
| **Skill Development/Performance** | Subordinate struggles with presentations                                                                            | "Your presentation lacked clarity, but I’ve seen you communicate well before. Let’s work on this."                                                                                                                                                                                                                                                                                                                         | Reassures them of their abilities while addressing gaps                                                                                                      | "That presentation wasn’t good at all. Fix it next time."                                                                                                                                                                                                                                                           | Help them rehearse or offer tips for the next presentation.                                                                                                        |
| **Information Sharing**           | A team member consistently misses key information shared in meetings and acts on outdated info.                     | "[Team Member Name], I've noticed a few times now you've seemed to miss crucial information shared in meetings, like the change in the project deadline we discussed on Monday. This has led to some rework. I know it can be a lot to keep track of, is there anything we can do to make sure you have the right information, like better note-taking or recaps?"                                                         | Focuses on the impact of the missed information (rework) rather than blaming the individual. Offers support and solutions.                                   | **Ruinous Empathy:**  Re-explaining the information every time without addressing the underlying issue. **Obnoxious Aggression:** "You never pay attention! Why are you always working with the wrong info?" **Manipulative Insincerity:**  Silently correcting their mistakes without ever addressing the pattern. | Offer to pair them with someone for meeting notes. Explore different communication channels for key updates.                                                       |
| **Change Management**             | An employee expresses resistance to a new process or tool being implemented.                                        | "[Employee Name], I understand you're feeling hesitant about using the new CRM system. You mentioned you prefer the old method because of [specific reason]. I hear that, and change can be tough. However, this new system will ultimately help us [explain benefits – e.g., streamline workflows, improve data accuracy]. Let's talk about your specific concerns and how we can make this transition smoother for you." | Acknowledges their feelings and validates their perspective. Explains the rationale behind the change and offers to address their specific concerns.         | **Ruinous Empathy:** Abandoning the new process because one person is resistant. **Obnoxious Aggression:** "Just do it! It's the new way, deal with it." **Manipulative Insincerity:**  Telling them it's their choice but then making it clear they are expected to use the new system.                            | Offer training and resources. Create a forum for feedback and address concerns. Highlight early successes with the new system.                                     |
| **Work Environment/Respect**      | A colleague has a persistent habit that annoys others (e.g., loud phone calls in a shared space).                   | "[Colleague Name],  I wanted to chat about something that's been a bit disruptive in our shared workspace. When you're on phone calls, especially the louder ones, it makes it difficult for others to concentrate. I understand you need to take calls, and perhaps we could explore a solution together, like using a meeting room or lowering your voice slightly?"                                                     | Focuses on the impact on others and the shared workspace. Offers collaborative problem-solving.                                                              | **Ruinous Empathy:**  Suffering in silence or complaining to others without addressing the colleague. **Obnoxious Aggression:** "Can you shut up on your calls? You're so loud!" **Manipulative Insincerity:**  Making sarcastic comments about their volume.                                                       | Suggest headphones for personal calls. Establish shared workspace etiquette.                                                                                       |
| **Problem Solving/Proactivity**   | A direct report consistently focuses on problems without offering solutions.                                        | "[Direct Report Name], I appreciate you bringing these challenges to my attention. While identifying problems is important, I'd also like to encourage you to think about potential solutions. Next time you bring up an issue, could you also come prepared with a couple of ideas on how we might address it?  This will help us move forward more effectively."                                                         | Acknowledges the value of identifying problems but gently redirects towards proactive problem-solving.                                                       | **Ruinous Empathy:**  Always jumping in to solve the problems for them. **Obnoxious Aggression:** "Stop complaining and just fix it!" **Manipulative Insincerity:**  Agreeing with their complaints but then doing nothing about them.                                                                              | Offer training on problem-solving methodologies. Praise and acknowledge when they offer solutions.                                                                 |
| **Teamwork/Contribution**         | A colleague isn't pulling their weight on a team project.                                                           | "[Colleague Name], let's touch base on the progress of the project. I've noticed that [specific task] hasn't been completed yet, and we're approaching the deadline. Is there anything I can do to support you in getting that done?  We need everyone contributing to ensure we meet our goals."                                                                                                                          | Focuses on the specific task and the impact on the project and team goals. Offers support rather than accusation.                                            | **Ruinous Empathy:**  Picking up their slack to avoid conflict or project delays. **Obnoxious Aggression:** "You're not doing anything! You're going to make us miss the deadline!" **Manipulative Insincerity:**  Complaining about their lack of contribution to other team members.                              | Clarify roles and responsibilities. Offer assistance or resources. Escalate to a manager if the issue persists.                                                    |
| **Meeting Preparation**           | An employee is consistently unprepared for meetings.                                                                | "[Employee Name], I've noticed in the last few meetings you haven't seemed to have reviewed the pre-reading materials or are unfamiliar with the agenda topics. This makes it harder for us to have productive discussions. Is there anything preventing you from preparing beforehand, or is there anything we can do to make the preparation process easier?"                                                            | Directly addresses the behavior and its impact on meeting productivity. Seeks to understand the reason and offer support.                                    | **Ruinous Empathy:**  Summarizing everything in detail for them in the meeting. **Obnoxious Aggression:** "Why are you so unprepared? You're wasting everyone's time!" **Manipulative Insincerity:**  Assigning them tasks they can't possibly complete without the meeting context.                                | Clearly communicate expectations for meeting preparation. Provide materials well in advance. Consider brief pre-reads together as a team.                          |
| **Meeting Participation**         | A team member has a habit of taking over conversations and dominating discussions.                                  | "[Team Member Name], you have valuable insights and I appreciate your contributions to our discussions. I've also noticed that sometimes it can be difficult for others to get a word in. Let's be mindful of creating space for everyone to share their perspectives so we can benefit from the full range of ideas in the team."                                                                                         | Acknowledges their positive contributions while directly addressing the negative behavior and its impact on inclusivity.                                     | **Ruinous Empathy:**  Saying nothing and letting them dominate, making others feel unheard. **Obnoxious Aggression:** "Stop talking! You're always talking!" **Manipulative Insincerity:**  Trying to subtly interrupt them without directly addressing the issue.                                                  | Use structured meeting formats that ensure equal participation. Gently redirect conversations if needed.                                                           |
| **Performance/Effort**            | A colleague consistently delivers work that is "good enough" but not their best effort.                             | "[Colleague Name], the work you submitted on the [project] is functional, and it meets the basic requirements. However, I know you're capable of producing higher quality work, and I've seen you do it before. Is there anything that's preventing you from putting in that extra level of detail or polish?  I want to see you succeed and produce your best work."                                                      | Acknowledges the minimum standard while expressing belief in their potential and a desire to see them excel.                                                 | **Ruinous Empathy:**  Praising mediocre work to avoid hurting their feelings. **Obnoxious Aggression:** "This is lazy! You didn't even try!" **Manipulative Insincerity:**  Giving vague feedback like "it's okay" without pointing out areas for improvement.                                                      | Provide specific examples of past high-quality work. Offer opportunities for skill development or mentorship.                                                      |
| **Feedback Receptiveness**        | An employee is resistant to feedback and becomes defensive when receiving constructive criticism.                   | "[Employee Name], I've noticed that when I offer suggestions for improvement, you sometimes seem to get a bit defensive. My intention is always to help you grow and develop your skills. It's important for us to be able to have open and honest conversations about how we can all improve. Is there a way I can frame feedback that would be more helpful for you?"                                                    | Focuses on their intention and the importance of feedback for growth. Opens a dialogue about how to deliver feedback more effectively for that individual.   | **Ruinous Empathy:**  Avoiding giving any feedback to prevent discomfort. **Obnoxious Aggression:** "You can't take criticism! You're too sensitive!" **Manipulative Insincerity:**  Giving positive feedback while secretly criticizing their performance to others.                                               | Schedule regular feedback sessions and create a safe space for discussion. Focus on specific behaviors and their impact.                                           |
| **Workload Management**           | A team member frequently complains about workload but doesn't proactively seek solutions or prioritize effectively. | "[Team Member Name], I hear you're feeling overwhelmed with your workload. It sounds stressful. Let's take a look at your current tasks and see if we can prioritize them more effectively, delegate some if possible, or identify any bottlenecks. Have you considered [specific time management technique]?"                                                                                                             | Acknowledges their feelings and validates their experience. Shifts the focus to proactive solutions and offers specific strategies.                          | **Ruinous Empathy:**  Taking on their work to alleviate their stress. **Obnoxious Aggression:** "Everyone is busy! Stop complaining and just get it done!" **Manipulative Insincerity:**  Agreeing they have too much work but then assigning them even more.                                                       | Provide training on time management and prioritization. Regularly review workloads and offer support in planning.                                                  |


---

| Category                | Scenario                                | Radical Candor                                                                                         | How It Shows Empathy?                                    | Alternate Statement or Course of Action                         | Follow-Ups                                                  |
|-------------------------|-----------------------------------------|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------|----------------------------------------------------------------|------------------------------------------------------------|
| **Performance Issues**  | Team member submits late work           | "I see the work is late, and I know you're capable of managing deadlines. Is there anything blocking you?" | Acknowledges their potential and offers support          | "This delay is unacceptable. Fix it."                       | Discuss their workload or obstacles to improve next time.  |
|                         | Frequent errors in a colleague's work   | "I see recurring errors in your work. Let’s identify causes and work on solutions together."            | Focuses on solving the problem collaboratively           | "You’re making too many mistakes. This isn’t acceptable."    | Review work quality after implementing solutions.          |
|                         | Employee delivers a confusing report    | "The report could be clearer in these areas. You’re great at structuring ideas; let’s refine this."     | Highlights their strengths and offers constructive help  | "This report is confusing and poorly done."                 | Review the revised report and provide further feedback.    |
|                         | Subordinate struggles with presentations | "Your presentation lacked clarity, but I’ve seen you communicate well before. Let’s work on this."       | Reassures them of their abilities while addressing gaps  | "That presentation wasn’t good at all. Fix it next time."    | Help them rehearse or offer tips for the next presentation. |
|                         | Team member avoids taking responsibility | "I noticed you deflected responsibility. I know you can take ownership. Let’s address this together."    | Expresses belief in their ability to improve             | "You always avoid accountability. Take responsibility."      | Follow up to see improved ownership in future tasks.       |
| **Team Dynamics**       | Peer dominates team discussions         | "You’re enthusiastic, but others need a chance to share too. Let’s work on balancing input."            | Values their passion but highlights the need for balance | "You need to stop talking so much in meetings."              | Observe future meetings and provide encouragement.         |
|                         | Colleague interrupts in meetings        | "I noticed you interrupt often, which might discourage others. Let’s give everyone space to speak."     | Addresses the issue while valuing their input            | "Stop interrupting everyone; it’s disruptive."              | Monitor discussions and praise balanced participation.     |
|                         | Colleague misses a key meeting          | "I noticed you missed the meeting. Is everything okay? Let’s plan how to keep you in the loop."         | Assumes good intent and cares about their well-being     | "You missed the meeting again. This can't happen."           | Share meeting notes and ensure clarity for future plans.   |
|                         | Team member seems disengaged            | "You seem less engaged lately. Is there something on your mind? How can I help?"                        | Shows concern for their emotional state                  | "Why aren’t you putting in effort lately?"                  | Schedule regular check-ins to maintain engagement.         |
|                         | Conflicts between team members          | "It seems like there’s tension between you and X. Can we work together to resolve this?"                | Mediates the issue while showing care for both sides     | "You two need to sort this out on your own."                | Facilitate a productive conversation to resolve conflicts. |
| **Recognition**         | Peer delivers exceptional work          | "This work is excellent! Your attention to detail really shows. Great job!"                             | Recognizes and appreciates their efforts                 | "Good work."                                                | Continue providing positive feedback when they excel.      |
|                         | Team member exceeds expectations        | "You’ve gone above and beyond on this project. It’s inspiring to see your dedication!"                  | Celebrates their effort and motivates them               | "Nice job on this project."                                 | Publicly acknowledge their contributions.                  |
|                         | New hire quickly adapts                 | "You’ve picked up things really fast for a new team member. Keep up the great work!"                    | Recognizes and encourages growth early on                | "You’re doing fine as a new hire."                          | Assign them more challenging tasks to foster growth.       |
|                         | Colleague supports others proactively   | "Your willingness to help others hasn’t gone unnoticed. It’s a great quality."                         | Validates their positive behavior                        | "Thanks for helping."                                       | Offer public acknowledgment in team meetings.              |
|                         | Team celebrates a major milestone       | "This achievement is a result of everyone’s hard work. I’m proud to be part of this team."              | Appreciates collective effort                            | "Good job on this milestone."                               | Plan a small celebration or thank-you note.                |
| **Behavioral Issues**   | Peer exhibits unprofessional behavior   | "Your behavior in the meeting came across as unprofessional. I know you can handle it better next time." | Addresses the issue without attacking the person          | "That behavior was unacceptable."                           | Offer coaching to improve their professionalism.           |
|                         | Team member shows up late repeatedly    | "I’ve noticed you’re often late. Is something affecting your schedule? Let’s find a solution."           | Offers understanding while addressing the issue          | "You’re always late. Stop doing that."                      | Track punctuality and reinforce positive habits.           |
|                         | Colleague gossips about others          | "Gossiping undermines team trust. Let’s focus on constructive discussions instead."                     | Encourages a healthier team environment                  | "Stop gossiping; it’s unprofessional."                      | Monitor behavior and reinforce positive communication.      |
|                         | Employee overpromises and underdelivers | "I see you’re taking on too much. Let’s prioritize your tasks to ensure success."                       | Offers support to manage workload                        | "You’re not delivering what you promised. Fix it."           | Revisit priorities and help manage expectations.           |
|                         | Team member reacts defensively to feedback | "I noticed you seem defensive about feedback. Is there a way I can make it easier for us to discuss this?" | Aims to understand their perspective                     | "You’re too defensive when I give feedback."                | Adjust feedback style and continue the conversation.       |
| **Growth & Development**| Employee struggles to meet goals        | "I see you’re having a hard time reaching your goals. Let’s break them down into smaller steps."         | Offers actionable guidance for improvement               | "You’re falling behind on your goals. Fix it."              | Set regular progress reviews to support improvement.       |
|                         | Team member lacks confidence            | "You’ve got the skills for this. Let’s focus on building your confidence to succeed."                   | Encourages them to believe in their abilities            | "You need to be more confident in your work."               | Assign tasks to gradually build their confidence.          |
|                         | Colleague wants more responsibility     | "I see you’re eager for more responsibility. Let’s discuss areas where you can grow."                   | Acknowledges ambition while setting clear expectations   | "I’ll let you know if something comes up."                  | Assign them new responsibilities and review their progress.|
|                         | Employee avoids taking feedback         | "I’ve noticed you avoid feedback. Let’s talk about why that might be and how we can make it easier."     | Seeks to understand their barriers to receiving feedback | "You never take feedback seriously."                        | Ensure feedback is framed positively and encourage dialogue.|
|                         | Peer shows strong leadership potential  | "You’ve shown leadership qualities in this project. Let’s build on that for bigger opportunities."       | Recognizes their potential and provides growth paths     | "You should lead more often."                               | Offer leadership roles and mentorship to develop skills.   |
