Finding "really good" semantic diffing papers requires looking at three distinct generations of research: the **structural** era (AST-based), the **semantic/behavioral** era (execution/verification-based), and the emerging **neural/LLM** era.

Here are the most relevant and impactful papers categorized by their approach to solving the problem.

### 1. The "Gold Standard" Structural Approaches
These papers are the foundation of modern semantic diffing. They moved the field beyond line-based diffs (like `git diff`) to tree-based diffs that understand code syntax.

*   **"Fine-Grained and Accurate Source Code Differencing" (ICSE 2014)**  
    *   **Authors:** Falleri et al.  
    *   **Why it's relevant:** This paper introduced **GumTree**, which is widely considered the state-of-the-art baseline for structural diffing. It treats code as an Abstract Syntax Tree (AST) and computes the shortest edit script (insert, update, delete, move) to transform one tree into another.  
    *   **Key takeaway:** If you build a tool today, this is the algorithm you are likely trying to beat. It excels at detecting moved blocks of code that standard text diffs miss.

*   **"Change Distilling: Tree Differencing for Fine-Grained Source Code Change Extraction" (IEEE TSE 2007)**  
    *   **Authors:** Fluri et al.  
    *   **Why it's relevant:** One of the earliest and most influential papers on **ChangeDistiller**. It defined a taxonomy of source code changes (e.g., "condition expression change" vs. "statement insert") that is still used to evaluate semantic precision today.

### 2. Modern Refactoring & Graph-Based Approaches
Standard AST diffs often fail when code is heavily refactored (e.g., extracting a method). These papers try to bridge that gap.

*   **"A Novel Refactoring and Semantic Aware Abstract Syntax Tree Differencing Tool" (arXiv 2024)**  
    *   **Authors:** Alikhanifard & Tsantalis  
    *   **Why it's relevant:** This is a very recent paper that explicitly addresses the limitations of GumTree. It integrates **RefactoringMiner** to help the diff algorithm understand high-level architectural changes (like "Extract Method") before attempting to match individual AST nodes. This results in much "smarter" mappings than pure tree matching.

*   **"ClDiff: Generating Concise Linked Code Differences" (ASE 2018)**  
    *   **Authors:** Huang et al.  
    *   **Why it's relevant:** This paper moves beyond just listing edit operations. It groups related low-level changes into higher-level "links" (e.g., grouping a variable declaration change with its usage updates). It’s excellent for generating human-readable semantic summaries.

### 3. Behavioral & Verification (True "Semantic") Diffing
"Semantic" can also mean "does the code *behave* differently?" These papers use symbolic execution rather than just syntax trees.

*   **"SymDiff: A Language-Agnostic Semantic Diff Tool for Imperative Programs" (CAV 2008 / Microsoft Research)**  
    *   **Authors:** Lahiri et al.  
    *   **Why it's relevant:** A classic paper on **Differential Symbolic Execution**. Instead of comparing text or trees, it translates two versions of a program into mathematical logic (via verification conditions) to prove if they are functionally equivalent or to find input values that cause them to diverge.

*   **"SemDiff: Binary Similarity Detection by Diffing Key-Semantics Graphs" (arXiv 2023)**  
    *   **Why it's relevant:** While focused on binaries, this represents a newer wave of using **Program Dependence Graphs (PDGs)** and symbolic execution to find "key semantics." It’s relevant if you are interested in how data flows change, rather than just how the code text changes.

### 4. The Frontier: LLM & Deep Learning Approaches
We are currently seeing a shift where LLMs are used to *explain* differences rather than strictly *compute* them.

*   **"SimLLM: Calculating Semantic Similarity in Code Summaries using a Large Language Model-Based Approach" (FSE 2024)**  
    *   **Why it's relevant:** This paper proposes using LLMs to measure the semantic similarity of code *summaries*, but the technique highlights how LLMs are being used to "understand" the semantic gap between two pieces of code better than traditional metrics like BLEU or standard AST edit distance.

*   **"Decompositional Semantic Analysis for LLM-based Code Quality Evaluation" (2024)**  
    *   **Why it's relevant:** This proposes decomposing code into semantic chunks (like "Assign", "While", "If") and using an LLM to analyze the semantic difference of each chunk independently. It’s a hybrid approach: using structural knowledge (ASTs) to guide the powerful but unpredictable "semantic intuition" of an LLM.

### Summary Recommendation
*   **For building a tool:** Start with **GumTree** (2014) as your engine.
*   **For handling refactors:** Read the **Refactoring and Semantic Aware** (2024) paper.
*   **For rigorous verification:** Look at **SymDiff** (2008).