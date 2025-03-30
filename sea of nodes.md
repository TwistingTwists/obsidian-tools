---
tags:
  - interview
  - breadth
  - compilers
other:
---
# Basics theory
### **What is a Sea of Nodes?**

Imagine a program as a set of **instructions** that have to run in a certain order. Normally, this order is based on how the program flows (one instruction after another), forming something called a **control flow graph (CFG)**.

But instead of thinking in terms of "what runs first and what runs next," the **Sea of Nodes** approach focuses on how **data moves** between instructions. It turns the program into a **graph of nodes**, where each node represents a calculation or operation. The only ordering it keeps is the one needed for **data dependencies** (i.e., "this calculation depends on that value").

This makes it easier for the **optimizer** to move things around and find the best way to run the program efficiently.

### **Why is it useful?**

- It helps **reorder** instructions more flexibly to optimize performance.
    
- It makes **dead code elimination** (removing useless code) and **constant propagation** (replacing variables with known values) work better together.
    
- It allows for **more aggressive optimizations**, which can lead to faster programs.
    

### **Where is it used?**

It’s used inside **high-performance compilers and JIT (Just-In-Time) compilers** like:

- **HotSpot** (used in Java)
    
- **GraalVM** (another Java-based compiler)
    
- **V8’s TurboFan** (used in Chrome’s JavaScript engine)
    
- **LibFirm** (a compiler backend for research and development)
    

### **Downside?**

Before actually running the program, the graph has to be converted back into a normal **control flow** structure using a **global code motion algorithm** (which is a fancy way of saying it has to figure out the best execution order).

### **Analogy**

Think of a traditional program as a list of cooking steps you must follow exactly. The **Sea of Nodes** approach, on the other hand, lets you think about **dependencies** instead—e.g., "I need chopped onions before I can fry them, but I can chop the garlic at any time." This flexibility lets a smart chef (the optimizer) reorganize the steps to make cooking faster.

Does that help? 😃

# V8's move to Maglev - CFG-IR

https://v8.dev/blog/leaving-the-sea-of-nodes?


# Basics again - making a Compiler with JIT