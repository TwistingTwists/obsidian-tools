Original question - 621 leetcode 

```python
from collections import Counter, deque
import heapq
from typing import List


class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        # Greedy simulation:
        # - Always run the currently available task with highest remaining count (max-heap).
        # - After running, put it into cooldown with a "ready_time".
        # - If nothing is available, CPU idles.

        if n == 0:
            return len(tasks)

        freq = Counter(tasks)

        # Python has a min-heap, so store negative counts for max-heap behavior.
        available = [-c for c in freq.values()]
        heapq.heapify(available)

        # cooldown holds entries: (ready_time, neg_count)
        cooldown = deque()

        time = 0
        while available or cooldown:
            time += 1

            # If a task is available, run the one with the largest remaining count.
            if available:
                neg_count = heapq.heappop(available)
                neg_count += 1  # we executed it once (e.g., -5 -> -4)

                # If it still has remaining instances, put it into cooldown.
                if neg_count != 0:
                    cooldown.append((time + n, neg_count))

            # Move tasks whose cooldown has expired back into the heap.
            # Note: multiple tasks can become ready at the same time.
            while cooldown and cooldown[0][0] == time:
                _, neg_count = cooldown.popleft()
                heapq.heappush(available, neg_count)

        return time
```
# variations 

Yep — “frame fitting / bucket filling” is the core idea, and a bunch of variants are basically: **what changes about the frame, and what counts as “filler”**?

Here are common extensions (and how the mental model adapts):

## 1) Variable cooldown per task

**Change:** each label `X` has its own cooldown `n[X]`.  
**Frame intuition:** there isn’t one global frame anymore; each high-frequency / high-cooldown task creates its own “spacing pressure.”  
**Typical solve:** simulation with a max-heap of available tasks + cooldown queue keyed by _ready time_.

## 2) Different task durations (processing time > 1)

**Change:** task takes `dur[X]` intervals to run, still needs cooldown after.  
**Frame intuition:** your “slots” are no longer unit-sized; gaps and fillers become variable-length.  
**Typical solve:** greedy simulation; sometimes becomes a scheduling / priority problem.

## 3) Weighted idles / cost minimization

**Change:** idling has cost, or tasks have penalties for lateness.  
**Frame intuition:** still gaps, but now you’re optimizing _which_ filler goes where to minimize cost.  
**Typical solve:** priority rules change (may become DP or more complex scheduling).

## 4) Multiple CPUs (k processors)

**Change:** you can run up to `k` tasks per interval; same-label tasks still must be ≥ n apart (across all CPUs).  
**Frame intuition:** the frame becomes “wider” each time unit (capacity `k`), and the bottleneck becomes how many distinct tasks you can place per interval without violating cooldown.  
**Typical solve:** heap + cooldown; counting formulas get trickier.

## 5) Return an actual schedule (not just length)

**Change:** output the sequence with idles, e.g. `"A B idle A B idle ..."`  
**Frame intuition:** same; you now have to _construct_ fillers in each gap.  
**Typical solve:** heap + cooldown naturally constructs a valid optimal schedule.

## 6) Tasks have release times (arrive over time)

**Change:** task instances aren’t all available at time 0.  
**Frame intuition:** you may not have enough filler _yet_, so you can be forced to idle earlier even if total work would have filled gaps.  
**Typical solve:** event simulation with arrivals + heap + cooldown.

## 7) Precedence constraints / DAG dependencies

**Change:** some tasks must happen before others.  
**Frame intuition:** your “filler pool” is restricted by what’s currently unlocked, so gaps may become unfillable.  
**Typical solve:** topological availability + heap + cooldown; may not have a simple formula.

## 8) Distance constraint generalization

**Change:** instead of “same label must be ≥ n apart”, you might have “A must be ≥ n away from B”, or a conflict graph.  
**Frame intuition:** becomes graph coloring over time / constraint satisfaction; “frame” is no longer just driven by one max-frequency class.  
**Typical solve:** greedy/heuristics; can become hard in general.

## 9) Cooldown window instead of exact spacing

**Change:** “in any window of length n+1, a label appears at most once” (equivalent to original), but sometimes extended to “at most r times.”  
**Frame intuition:** buckets now allow limited repeats; filler capacity changes.  
**Typical solve:** counting / greedy with quotas.

---

### Why the original has a clean formula (and many extensions don’t)

The original works neatly because:

- unit time per task
- single global cooldown `n`
- no release/dependency constraints
- objective is just minimize makespan

That combination makes the “most frequent tasks define the frame” argument airtight. As soon as availability or constraints vary, you usually lose the closed form and fall back to **heap + cooldown simulation**.

If you tell me which extension you’re most interested in (multi-CPU? variable cooldown? output schedule?), I’ll show how the “frame fitting” intuition morphs and what the canonical solution looks like.