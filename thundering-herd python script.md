Here you go — this script simulates exactly the “**trickle** adds 1 connect when `in_flight > pending`” idea, and plots it against a “**naive** start-as-many-as-possible” policy.
![[3699f0f4905bd347621c6d875053f75ea64d6bcfd294746242453545a993e33e.png]]



![[bc0b22b9b47ecae8781a19b6bd429cdca97b3a73deb1e24a381d92e711d12f8b.png]]It 




![[2eb3e55fbec0248f39f7f8aea3abe9255417fb2d43202d050647458b9a33e606.png]]

generates 3 plots:

1. Trickle: demand vs pending vs created
    
2. “Connect storm” per tick: trickle vs naive
    
3. Pending over time: trickle vs naive
    

```python
import numpy as np
import matplotlib.pyplot as plt

# Simulate "trickle" approval policy: if in_flight > pending, start 1 new connect per tick.
# Also include a naive policy for comparison: start as many connects as waiting tasks (up to max_size).

def simulate(trickle=True, ticks=60, arrival_at=5, burst=50, max_size=20, connect_latency=8):
    """
    Returns time series dict with keys:
      t, in_flight, pending, created, started_connects, completed_connects
    """
    in_flight = 0
    pending = 0
    created = 0

    # pending connections are those in "connecting" state with remaining latency
    connecting = []  # list of remaining ticks

    ts = {
        "t": [],
        "in_flight": [],
        "pending": [],
        "created": [],
        "started_connects": [],
        "completed_connects": [],
    }

    for t in range(ticks):
        # arrivals (a burst of concurrent getters)
        if t == arrival_at:
            in_flight += burst

        # progress existing connects
        completed = 0
        new_connecting = []
        for rem in connecting:
            rem -= 1
            if rem <= 0:
                completed += 1
            else:
                new_connecting.append(rem)
        connecting = new_connecting

        # On completion: pending decreases, created increases.
        if completed:
            completed = min(completed, pending)  # safety
            pending -= completed
            created += completed

        # Decide how many new connects to start this tick
        available_capacity = max_size - (created + pending)
        available_capacity = max(0, available_capacity)

        if trickle:
            start = 1 if (in_flight > pending and available_capacity > 0) else 0
        else:
            # naive: try to cover all waiting now (still capped by max_size)
            start = min(in_flight, available_capacity)

        # Start new connects
        if start:
            pending += start
            connecting.extend([connect_latency] * start)

        # record
        ts["t"].append(t)
        ts["in_flight"].append(in_flight)
        ts["pending"].append(pending)
        ts["created"].append(created)
        ts["started_connects"].append(start)
        ts["completed_connects"].append(completed)

    return ts


# ---- parameters to tweak ----
TICKS = 60
ARRIVAL_AT = 5
BURST = 50
MAX_SIZE = 20
CONNECT_LATENCY = 8
# -----------------------------

tr = simulate(trickle=True, ticks=TICKS, arrival_at=ARRIVAL_AT, burst=BURST,
              max_size=MAX_SIZE, connect_latency=CONNECT_LATENCY)
nv = simulate(trickle=False, ticks=TICKS, arrival_at=ARRIVAL_AT, burst=BURST,
              max_size=MAX_SIZE, connect_latency=CONNECT_LATENCY)

# Plot 1: trickle demand vs supply in progress vs supply ready
plt.figure()
plt.plot(tr["t"], tr["in_flight"], label="in_flight (demand)")
plt.plot(tr["t"], tr["pending"], label="pending (connecting)")
plt.plot(tr["t"], tr["created"], label="created (ready)")
plt.title("Trickle policy: in_flight vs pending/created")
plt.xlabel("tick")
plt.ylabel("count")
plt.legend()
plt.show()

# Plot 2: connect storm comparison
plt.figure()
plt.plot(tr["t"], tr["started_connects"], label="started connects (trickle)")
plt.plot(nv["t"], nv["started_connects"], label="started connects (naive)")
plt.title("Connect storm comparison: trickle vs naive")
plt.xlabel("tick")
plt.ylabel("new connects started per tick")
plt.legend()
plt.show()

# Plot 3: pending over time
plt.figure()
plt.plot(tr["t"], tr["pending"], label="pending (trickle)")
plt.plot(nv["t"], nv["pending"], label="pending (naive)")
plt.title("Pending connections over time")
plt.xlabel("tick")
plt.ylabel("pending")
plt.legend()
plt.show()

print("Params:", {"burst": BURST, "max_size": MAX_SIZE, "connect_latency": CONNECT_LATENCY})
print("Trickle peak started per tick:", max(tr["started_connects"]))
print("Naive peak started per tick:", max(nv["started_connects"]))
print("Trickle time to reach max created:", next((t for t,c in zip(tr["t"], tr["created"]) if c==MAX_SIZE), None))
print("Naive time to reach max created:", next((t for t,c in zip(nv["t"], nv["created"]) if c==MAX_SIZE), None))
```

What you should see:

- **Naive**: a giant spike starting up to `MAX_SIZE` connects immediately (storm).
    
- **Trickle**: starts **1 per tick**, so pending grows gradually; created ramps up smoothly.
    

If you want it to look even closer to the actual pool behavior, tell me whether you want the simulation to include “idle queue” and “demand being satisfied reduces in_flight” (right now in_flight stays high just to visualize the ramp clearly).