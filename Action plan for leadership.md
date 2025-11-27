Here are tailored answers for software engineering managers, followed by a self-assessment framework:

---

1. The Hierarchy Test
Q: Tell me about a time when you had to make a decision that benefited your customer but hurt your team's morale or your own career goals.

Example Answers:
- "Our enterprise client needed a custom API integration in 2 weeks that required my team to scrap a sprint. I took the hit: I personally worked nights/weekends, shielded my team from the worst of it, and we delivered. The client renewed for 3 years. My promotion got delayed because I missed internal KPIs that quarter, but I modeled what 'Customer > Me' actually means."
- "A customer bug was trending on Twitter. My team was building a feature I needed for my performance review. I redeployed the team, we fixed it in 4 hours, then I explained to leadership why my goals slipped. The customer success team sent us a direct thank-you video from the CTO that I showed my team—that became their morale boost, not the feature."
- "I removed myself from the promotion pipeline to advocate for my team's budget to fix technical debt that was slowing down a Fortune 500 customer. I told my manager: 'My promotion can wait; their SLA can't.' Six months later, that customer doubled their contract, and leadership promoted me anyway for showing ownership."

---

2. The "Pick Up the Broom" Test
Q: Describe a situation where you had to do work "beneath your title" to serve a customer or solve a critical problem.

Example Answers:
- "As an engineering manager, I debugged a P1 outage at 2 AM that turned out to be a DNS misconfiguration. My senior engineer was burned out. I screenshared from my phone while on a family vacation, fixed it in 20 minutes, then documented the fix in our wiki at 3 AM. The next day, I told my team: 'Never assume you're above the work.'"
- "Our junior engineer was struggling with a complex React bug that was blocking a customer demo. I spent 3 hours pair-programming with them, not just watching but actually typing alongside them. We fixed it, and they told me later that was the moment they felt safe to ask for help. Leadership isn't abdicating the keyboard."
- "I cleaned up our lab environment and refilled coffee for a customer workshop because our ops person called in sick. The customer saw me doing it and said: 'That's how I know your company actually cares.' My team now takes ownership of the office space because they saw me literally pick up the broom."

---

3. The High Trust vs. High Control Dilemma
Q: Have you worked in a high-trust environment? How did you know trust existed, and what did you do when you saw a problem in another team?

Example Answers:
- "At my last startup, we had a 'no PR approval needed for hotfixes' policy. That trust was earned: when I saw the mobile team's API causing latency for my web team, I didn't file a ticket—I walked over, paired with their engineer for 2 hours, and we shipped a fix together. No meetings, no blame. The lead thanked me for 'passing the ball, not the buck.'"
- "High trust means my director let me hire without his final approval because I'd proven my bar. When I noticed the infra team's deployment pipeline was breaking our builds, I didn't escalate—I joined their standup, offered to help refactor their Jenkins scripts, and we co-owned the solution. Trust = I could see their problem as my problem."
- "We measured trust by how quickly bad news traveled. When I saw the QA team was burning out and cutting corners, I didn't gossip—I invited their manager to coffee, shared my observations with data, and offered two of my engineers to help automate their tests. High trust means no 'your team/my team'—just 'our customer.'"

---

4. The "Dive Into Details" Test
Q: Tell me about a time when staying surface-level would have been easier, but you chose to go deep into the details.

Example Answers:
- "My team was building a microservice and wanted to use a new database. Instead of rubber-stamping it, I spent a weekend reading the source code, found a critical bug in their transaction handling, and had them switch. The CTO later told me that decision saved us a 500K outage. I earned their respect by diving deep, not coasting on my title."
- "A junior engineer's PR looked fine at a glance, but I pulled the branch and ran it locally. I found a race condition that would have caused 1% of users to see corrupted data. I paired with them for 4 hours to fix it and write a test. They told me: 'I thought you were just going to approve it because you're busy.'"
- "We had a mysterious memory leak in production. I could have delegated, but I spent 3 days profiling heap dumps, learning Rust's memory model, and discovered we were cloning massive data structures. The fix took 2 lines. My team's velocity doubled after that because they saw their manager would do the gritty work."

---

5. The "Sub-Optimal Decision" Test
Q: Describe a time when you had imperfect information but had to make an irreversible (Type 2) decision.

Example Answers:
- "I had to decide whether to hire a senior engineer with a great resume but yellow flags on collaboration. I gathered 5 backchannel references, found a pattern of territorial behavior, and rejected them despite pressure to fill the role. Three months later, we found someone better. The cost of a sub-optimal hire is 10x the cost of waiting."
- "We needed to choose a cloud provider before our Series B. I had 48 hours. I wrote a 1-page decision doc with assumptions, risks, and a 90-day checkpoint. We chose AWS. At 90 days, we discovered our workload was cheaper on GCP—so I publicly admitted my mistake and we migrated. No ego, just optimal decisions."
- "My team wanted to adopt a new framework. I knew it was Type 2 (irreversible). I made them run a 2-week spike, present to the entire engineering org, and get 3 dissenting opinions before I approved. It delayed us by a month, but the scrutiny ensured it was optimal, not just popular."

---

6. The "Kind vs. Nice" Feedback Test
Q: Tell me about the hardest piece of feedback you've given. How did you balance directness with care?

Example Answers:
- "I had an engineer who was brilliant but toxic in code reviews—publicly shaming juniors. I told them: 'Your technical skills are top 5%, but your behavior is bottom 50%. If this doesn't change in 30 days, I'll remove you from the team. I'll help you, but I will not sacrifice the team's psychological safety.' They changed. They're now a staff engineer."
- "A senior engineer on my team wasn't coding anymore—just architecture meetings. I said: 'I see you as a player-coach. Right now, you're only coaching. Your team is losing respect because they see you're not in the code with them. I need you to ship 3 features this quarter, or we'll discuss a different role.' They stepped up."
- "I had to fire an engineer who was trying hard but couldn't meet our bar. I spent 6 hours over 2 weeks helping them find a new role elsewhere, practice interviews, and even connected them to a company where they'd thrive. Kind = I want you to succeed, even if it's not here. Nice would have been letting them flounder for another year."

---

7. The Global Remote Sacrifice Test
Q: Working globally requires sacrifices. Tell me about a time you chose between personal convenience and global team collaboration.

Example Answers:
- "I'm in SF; my team is in Bangalore. I blocked 7-9 AM daily for 'AST' (Atlan Standard Time) and told my family I'd miss breakfast with them. I also blocked 6-8 PM twice a week for optional deep work with my night-owl engineers. My wife wasn't thrilled, but I explained: 'This is the cost of building a global championship team.' I make it up by being fully present on weekends."
- "I was on vacation in Hawaii when our customer in Germany had a P1. It was 4 AM my time. I joined the war room, not to micromanage, but to unblock decisions and show the customer we cared. I then slept 4 hours, spent the day with my family, and did a handoff at 10 PM. The customer renewed because they saw we never clock out on them."
- "I moved my entire team's standup to 6:30 AM Pacific so our India team could end their day by 8 PM IST. I lost an hour of sleep, but my team's talent density increased because we could hire the best people in India, not just those willing to burn out. That's picking 'Company > Me.'"

---

8. The "Talent Density" Ownership Test
Q: Walk me through how you'd design a hiring process that maintains an extremely high bar while creating an exceptional candidate experience.

Example Answers:
- "I design a 4-hour take-home challenge that mirrors our actual codebase. I personally review every submission, spending 30-60 minutes on each. I send personalized feedback to every candidate, even rejected ones. One candidate I rejected tweeted about our process—got us 10 more applicants. High bar + high care = flywheel."
- "I calibrate by interviewing 5 exceptional people in my network before opening a req. I ask them to do our challenge so I know what 'great' looks like. I then involve 2 engineers, 1 designer, and 1 PM in every onsite—cross-functional evaluation. We've rejected 20 candidates who met every skill but failed our 'no-gossip' culture screen. Talent density > speed."
- "I run the hiring funnel like a product: I track conversion rates, drop-off points, and candidate NPS. When I saw senior engineers dropping after the challenge, I added a 15-minute pre-challenge call to explain why it matters. Our completion rate went from 60% to 85%. The bar stayed high; the experience got human."

---

9. The "Crucial Conversation" Test
Q: Tell me about a time you had a crucial conversation where stakes were high, opinions varied, and emotions were strong.

Example Answers:
- "My team wanted to rewrite our monolith in microservices; I thought it was premature. I scheduled a 2-hour debate where I forced them to argue against microservices and me for it. We flipped roles. They realized the ops overhead; I saw their scaling pain. We compromised: extract one service first. The process built trust because we attacked the problem, not each other."
- "A top-performing engineer was bypassing code review for speed. I told them: 'You're costing us trust. One more time, and I remove your merge access. This isn't about you—it's about the 5 juniors who think rules don't apply to stars.' They got it. We created a 'fast-track review' process for them to mentor others instead."
- "I had to tell my director that his pet project was killing team morale. I brought data: 40% of our sprint was his 'experiments,' and our customer bug fix rate dropped 60%. I said: 'I respect your vision, but your method is breaking the team. Let's find another way.' He killed the project. It was tense, but I earned his respect for speaking truth."

---

10. The "Ikigai & Tours of Duty" Test
Q: Tell me about a time when your personal career aspirations didn't align with what your company needed.

Example Answers:
- "I wanted to manage a 20-person team, but the company needed me to be an IC staff engineer to fix our data pipeline crisis. I took the Tour of Duty, spent 6 months in the trenches, and shipped a system that processed 10B events/day. After, they gave me a 15-person team and a staff title. The company remembers who says yes."
- "My career plan was frontend architect, but we lost our backend lead. I told my manager: 'I'll own the backend for 12 months, but I need you to commit to rotating me back to frontend after.' He agreed. I built a team, shipped our API platform, and at 12 months, he made me full-stack architect—better than my original plan."
- "I was a manager but realized I missed coding. The company needed a strong IC in ML infrastructure. I stepped down from management (worried about optics), spent a year shipping models, and now I'm a Staff ML Engineer with more influence than I had as a manager. My resume says 'Staff'—the tour was the best career move I made."

---

11. The "No-Gossip" Culture Test
Q: Describe your team culture. What specifically did you do to prevent passive disagreement and gossip?

Example Answers:
- "I instituted a rule: venting must end with an action. If someone complained about another team, I'd ask: 'What will you do about it?' If they said 'nothing,' I'd say: 'Then don't waste energy complaining.' It killed gossip instantly. People either solved it or stayed quiet."
- "In our Slack, we have a #no-stupid-questions channel where I post my own mistakes weekly. When someone passive-aggressively eye-rolled about our QA team in a meeting, I stopped and said: 'Let's say that directly to them. I'll set up the meeting for you.' They apologized. The behavior stopped."
- "I modeled it: when a PM told me privately they thought my engineer's estimate was inflated, I pulled both into a room and said: 'Talk this through now. I'll moderate.' The PM learned the complexity, the engineer learned to explain better, and they built trust. No scar tissue."

---

12. The "Slope > Y-Intercept" Learning Test
Q: How have you upgraded your leadership "mech suit" in the last year? What feedback did you act on?

Example Answers:
- "My team told me I was 'a helicopter manager'—always hovering. I got a coach, set up a weekly 1:1 with my director for feedback, and now I do 'no-meeting Wednesdays' where I don't check code or Slack. My team's velocity increased 30%. The feedback was a gift; I treated it as upgrading my suit, not attacking me."
- "A peer told me my code reviews were too harsh—engineers were scared to merge. I recorded myself reviewing and counted my negative comments: 85% critical. I rewrote my style to include 3 positives per negative, added 'appreciation comments,' and now my team merges 2x faster. I was crushing their confidence, not raising the bar."
- "I realized I was avoiding learning Rust because I was 'a manager now.' I spent 3 months building a side project in Rust, blogged about my failures, and now I can actually assess my team's technical decisions. My 'y-intercept' was high as a former Python expert, but my slope was flat. That had to change."

---

Self-Assessment & Improvement Framework

How to Test Your Current Level

Scorecard Audit (Rate 1-5 for each):
1. Hierarchy Living: When's the last time you chose customer/company over your team/me? Check your calendar: How many hours this week were spent on internal politics vs. customer-impacting work?
2. Broom Moments: How many times this month did you do IC-level work to unblock your team? If zero, you're too removed.
3. Trust Metrics: Count how many cross-team problems you solved without involving managers. If you're waiting for permission, you don't trust yet.
4. Details Depth: Can you explain your team's top 3 technical risks right now? Not from a report—from memory, with specifics?
5. Decision Quality: Look at your last 5 irreversible decisions. How many times did you change your mind based on new data? Zero = ego problem.
6. Feedback Frequency: Count your "crucial conversations" this quarter. If it's less than 1 per direct report, you're being nice, not kind.
7. Global Empathy: How many times have you taken a call at an inconvenient time without complaining? Your team sees your sacrifice.
8. Hiring Bar: Of your last 10 candidates, how many did you reject despite pressure to fill the role? If <30%, you're compromising.
9. Gossip Detection: Ask your team: "Where have you seen me shut down passive disagreement?" If they can't answer, you're not modeling it.
10. Learning Velocity: What new technical or leadership skill have you demonstrated in the last 3 months? Not read about—actually done.

---

Action Items to Measure Progress

Week 1: Baseline
- Action: Ask your team: "On a scale of 1-10, how much do you see me living 'Customer > Company > Team > Me'?" Average = your score.
- Action: Record yourself in a meeting. Count how many times you say "I think" vs. "What does the customer need?"
- Measure: Track your time: % spent on customer-impacting work vs. internal process.

Month 1: Behavioral Shifts
- Action: Do one "broom" task weekly (debug, doc, deploy). Document it in a public Slack channel. Track team engagement increase.
- Action: Initiate one cross-team problem-solving session without managers. Measure time-to-resolution.
- Action: Have one "kind, not nice" conversation per week. Follow up: Did they improve? Did they thank you later?
- Measure: Net Promoter Score from your team: "How likely are you to recommend me as a leader?" Track weekly.

Quarter 1: Systemic Changes
- Action: Redesign your hiring process: Add a challenge, calibrate with 5 external experts, track candidate NPS. Target: 80+ NPS, <10% offer rate.
- Action: Create a "no-gossip" team norm. First violation: public reminder. Second: 1:1 crucial conversation. Third: removal. Track violations.
- Action: Take 3 calls in "AST" (Atlan Standard Time) at personal inconvenience. Don't mention it. See if your India team starts mirroring your behavior.
- Measure: Technical audit: Can you explain your team's architecture to a new hire without preparation? Record yourself. If >3 "ums," you need to dive deeper.

Ongoing: Learning Velocity
- Action: Publish one "mech suit upgrade" per month: blog about a mistake, teach a lunch-and-learn on a new skill, or ask for feedback publicly.
- Action: Find a "thought partner" outside your chain of command. Weekly 30-min call to pressure-test your decisions. Track decisions changed.
- Measure: Slope > Y-Intercept metric: Rate your own learning velocity monthly. If it's flat for 2 months, you're falling behind Atlan's pace.

---

Red Flags: You're Failing
- You haven't touched code in 6 months.
- Your team only brings you good news.
- You've never changed your mind publicly.
- You can't name your top performer's low-energy tasks.
- You've compromised on a hire because "we needed someone fast."
- You think "managing up" is part of your job.

Green Flags: You're Succeeding
- Your team gives you unsolicited feedback.
- You rejected a candidate who met all skills but failed culture fit.
- You've taken a 6 AM call and didn't tell anyone.
- You've publicly admitted a wrong decision and changed course.
- Your engineers are solving cross-team problems without you.

Final Test: Ask your team: "If I got hit by a bus tomorrow, what would you remember me for?" If the answer is "your title" or "your meetings," you're not leading Atlan's way. If it's "the time you debugged with me at midnight" or "the hard conversation that made me better," you're building the culture this handbook demands.




---



Here's a short note on how you can test your awareness of these topics and create an action plan for improvement.
🧪 How to Test Your Current Awareness (Self-Assessment)
The goal is to find the gaps between your automatic instincts and the principles in the handbook.
 * Log Your Dilemmas: For one week, keep a private log of every leadership dilemma you face.
   * Test: At the end of the day, review your log. When you had to choose between a customer request and your team's comfort, what did you actually do? Compare your actions against the Customer > Company > Team > Me hierarchy. The gaps are your areas for focus.
 * Audit Your "Avoided" Conversations: Make a list of all the difficult, "kind" conversations you are currently putting off—whether it's about performance, feedback, or a personal conflict.
   * Test: The length of this list is the test. A long list shows you're defaulting to being "nice" (avoiding conflict) rather than "kind" (driving growth).
 * Run a "Blame" Diagnostic: Think of the last major cross-functional problem.
   * Test: Be brutally honest: Was your first instinct to figure out how the other team dropped the ball? Or was it to ask, "How can my team and I help solve this?". The former shows a "pass the buck" mindset; the latter is the "pass the ball" standard.
 * Scan for "Brooms": Look at your team's recent work.
   * Test: How many times did you dive deep into the details (like code, specs, or support tickets) versus just managing from a distance?. If you're never in the details, you're not "picking up the broom".
📈 How to Improve and Measure Progress (Action Plan)
 * Schedule Your "Kindness"
   * Action: Take one item from your "Avoided Conversation" list. Schedule that 30-minute feedback session for tomorrow. Prepare for it by writing down specific examples, coming from a place of "true care".
   * Measure Progress: Your "Avoided" list gets shorter. You'll measure success not by whether the conversation was "easy," but by whether you had it and were both honest and caring.
 * Appoint a "Hierarchy Keeper"
   * Action: In your next team meeting where you're setting priorities, explicitly state the Customer > Company > Team > Me hierarchy. Ask one team member to be the "Hierarchy Keeper" for that meeting, with the power to flag any decision that seems to violate it.
   * Measure Progress: Your team starts making faster, more aligned decisions. You'll hear them use the hierarchy in debates without you prompting them.
 * Ask for "Mech Suit" Feedback
   * Action: The handbook suggests your leadership style is a "mech suit" you can upgrade. Go to one peer and one direct report. Ask them, "What is one thing I should stop doing to be a more effective leader?".
   * Measure Progress: 1) You successfully ask for and receive the feedback without getting defensive. 2) The next month, you can ask them, "I've been working on [the feedback]. Have you seen an improvement?"
 * Log Your "Broom" Actions
   * Action: For one week, make it a point to do one "pick up the broom" task each day that is "beneath" your title—whether it's debugging code, cleaning up documentation, or personally handling a minor customer ticket.
   * Measure Progress: Your team sees you "in the trenches". You'll find your context on real-world problems improves, and your team's respect for you will grow.


---

