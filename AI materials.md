---
tags:
  - materials
  - repository
---

## CUDA 
jeremy howard
https://www.youtube.com/watch?v=nOxKexn3iBo

https://github.com/cuda-mode/lectures/blob/main/lecture3/pmpp.ipynb

### MCTS
[source1](https://www.youtube.com/watch?v=hmQogtp6-fs)

Monte Carlo Tree Search (MCTS) is especially versatile because it does not require a hand-crafted heuristic—only a way to simulate outcomes from a given state. This makes MCTS suitable for a wide range of decision-making and planning tasks, such as:

1. **Board and Video Games**
    
    - **Go and Chess:** AlphaGo famously combined deep neural networks with MCTS to surpass top human players.
    - **General Game-Playing:** Any turn-based board game (e.g., Checkers, Othello) can benefit from MCTS if you can simulate possible moves from a given board state.
    - **Video Games:** MCTS can be used to control non-player characters (NPCs) or handle strategic decision-making in complex real-time or turn-based video games.
2. **Automated Planning and Scheduling**
    
    - **Resource Allocation:** Where you need to balance multiple resources under constraints and want to explore many potential strategies.
    - **Scheduling & Dispatching:** For factories, logistics, or airline schedules, MCTS can simulate various scheduling actions and compare outcomes (e.g., on-time performance, costs).
3. **Robotics**
    
    - **Motion Planning and Control:** In complex environments, MCTS can be used to pick from various robot actions (move/rotate/pick/place), simulate the resulting paths in a simulator, and choose actions that maximize safety or efficiency.
    - **Task Planning:** For multi-step tasks, MCTS can evaluate different sequences of actions, especially when actions have uncertain outcomes.
4. **Financial or Strategic Decision Analysis**
    
    - **Portfolio Optimization:** If you can model potential market states or payoffs, MCTS can help explore different allocation strategies and converge on a strong investment decision.
    - **Risk Management:** MCTS can simulate scenarios in which various events occur (e.g., interest rate changes, market shocks) and then guide decisions to minimize losses.
5. **Complex Scientific/Engineering Domains**
    
    - **Drug Discovery or Materials Design:** MCTS can be combined with simulation software (e.g., docking simulations for drug molecules) to iteratively propose variations, simulate outcomes, and identify promising leads.
    - **Energy Grid Management:** Where you have multiple sources (renewables, thermal plants) and demand levels, MCTS can help evaluate different dispatch strategies under uncertainty.
6. **Game Theory and Multi-Agent Simulation**
    
    - **Negotiation or Auctions:** MCTS can be extended to handle multi-agent interactions, simulating how opponents or other agents might behave.
    - **Policy Design:** In areas like traffic control, MCTS can help weigh different policies for traffic lights or tolling, using traffic simulations to see which policy yields the best flow.

---

### Why MCTS Excels in These Areas

- **No Need for a Custom Heuristic:** MCTS evaluates moves by _simulating_ outcomes rather than relying solely on handcrafted heuristics.
- **Flexible and General:** It can be applied to almost any state-based model where you can simulate forward from a given state to an end condition.
- **Adaptive and Anytime:** MCTS refines its strategy as more computations occur. Even if you can’t fully “solve” a domain, MCTS can provide a best guess quickly and keep improving the longer it runs.

Overall, MCTS is best for **complex, high-dimensional problems** where enumerating all possibilities is impossible, but you can **simulate** how a choice might unfold—and learn from that simulation data.
