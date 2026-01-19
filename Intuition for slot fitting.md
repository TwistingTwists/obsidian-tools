From slots → “pick most frequent available”
Once you accept the idea of filling holes, the next question is:
If I want to avoid idles, which tasks should I use to fill the holes?
Answer: use tasks that are most likely to cause trouble later — i.e., tasks with highest remaining frequency.
Because if you postpone a high-frequency task, you increase the chance you’ll later be stuck with it while it’s on cooldown → leading to idle.
That greedy principle is the bridge to a max heap:
“always pick the task with largest remaining count among those currently allowed”
This is the same intuition as “rearrange string so no adjacent duplicates”: always place the most frequent next.
Cooldown is just “adjacent duplicates” generalized to distance n.


Why priority queue specifically?
Because at each time step you need:
find the available task with max remaining count (fast)
update its count
temporarily remove it for cooldown
reinsert it when cooldown expires
That’s literally:
a max heap for available tasks (key = remaining count)
a queue (often FIFO) of cooling tasks storing (ready_time, count, label)
So the natural data structures fall out of the required operations.




How to train your eye to spot “slots vs PQ”
A quick checklist while reading:
“Slots / frames” model triggers
constraint is purely about distance between identical items
objective is min total length
counts matter more than identities
→ think: “most frequent item creates mandatory gaps”
“PQ + cooldown” model triggers
you can choose any task each time
some tasks are temporarily unavailable
you need repeated “pick best available now”
→ think: “available set changes over time” → heap + cooldown structure



Connect slots and PQ (so it feels like one idea)
They’re the same story at different zoom levels:
Slots formula is the “global counting proof” based on the max frequency forcing gaps.
PQ simulation is the “constructive greedy schedule” that actually builds one optimal schedule and counts intervals.
If you can justify the slots lower bound, PQ gives you a way to meet it (or match m when no idles needed).



