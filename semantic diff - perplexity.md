
# Semantic Code Diff: Key Research Papers and Approaches

Semantic code differencing goes beyond traditional line-based diffs to understand the **meaning** and **behavior** of code changes. Here are the most relevant papers and tools in this space, organized by approach:

## Foundational Work: Program Dependence Graphs

**Susan Horwitz (1990) - "Identifying the Semantic and Textual Differences Between Two Versions of a Program"**[acm+1](https://dl.acm.org/doi/abs/10.1145/93548.93574)​

This seminal paper introduced a technique using **program dependence graphs (PDGs)** to compare programs semantically rather than textually. Horwitz's approach determines which components represent actual behavioral changes versus mere syntactic modifications. The key insight is that two programs are semantically identical if they produce the same sequence of observable values, even if their syntax differs.[gmu+1](https://cs.gmu.edu/~white/CS640/10.1.1.50.4405.pdf)​

The PDG explicitly encodes **control dependences** (what statements affect which others' execution) and **data dependences** (flow of values between statements), enabling the algorithm to identify changes that actually affect program behavior while ignoring cosmetic changes like variable renaming or code reformatting.[research.wisc+1](https://research.cs.wisc.edu/wpis/papers/icse92.pdf)​

## AST-Based Differencing

**GumTree (2014) - "Fine-grained and Accurate Source Code Differencing"** by Falleri et al.[acm+1](https://dl.acm.org/doi/10.1145/2642937.2642982)​

GumTree is arguably the most influential AST differencing tool, with over 800 citations. It pioneered computing edit scripts at the **abstract syntax tree granularity** including **move actions**, which line-based tools cannot detect. The algorithm works in two phases: a **top-down** phase that matches isomorphic subtrees starting from roots, followed by a **bottom-up** phase that matches remaining nodes based on similarity of their descendants.[courses.vt+2](https://courses.cs.vt.edu/cs6704/spring17/slides_by_students/CS6704_gumtree_Kijin_AN_Feb15.pdf)​

**ChangeDistiller (2007) - "Change Distilling: Tree Differencing for Fine-Grained Source Code Change Extraction"** by Fluri et al.[ieeexplore.ieee+1](https://ieeexplore.ieee.org/document/4339230/)​

This paper improved hierarchical change detection by using **bigram string similarity** for matching code statements and adapting tree edit distance algorithms for ASTs. ChangeDistiller extracts fine-grained change types according to a taxonomy of source code changes, enabling precise identification of statement-level modifications.[notes.billmill+2](https://notes.billmill.org/images/Change_DistillingTree_Differencing_for_Fine-Graine.pdf)​

**MTDIFF (2016) - "Move-optimized Source Code Tree Differencing"** by Dotzler et al.[acm+2](https://dl.acm.org/doi/10.1145/2970276.2970315)​

MTDIFF introduced five optimizations to improve **move detection** accuracy over GumTree. It addresses cases where code blocks are relocated within a program, producing shorter and more accurate edit scripts.[xifiggam+1](https://www.xifiggam.eu/wp-content/uploads/2018/08/GeneratingAccurateandCompactEditScriptsusingTreeDifferencing.pdf)​

**IJM - Iterative Java Matcher**[acm+2](https://dl.acm.org/doi/10.1145/3696002)​

IJM enhances GumTree by **merging nodes**, **pruning sub-trees**, and refining the diffing algorithm. Studies show it provides better accuracy for move and update actions compared to GumTree and MTDIFF, particularly for understanding complex refactorings.[arxiv+2](https://arxiv.org/html/2411.07718v1)​

## Refactoring-Aware Differencing

**RefactoringMiner-based AST Diff (2024) - "A Novel Refactoring and Semantic Aware Abstract Syntax Tree Differencing Tool"** by Alikhanifard and Tsantalis[arxiv+1](https://arxiv.org/abs/2403.05939)​

This recent work addresses critical limitations of existing tools: lacking **multi-mapping support**, matching semantically incompatible nodes, and not being **refactoring-aware**. By building on RefactoringMiner, this tool can detect when a change represents a refactoring (like extract method or rename) and present the diff accordingly. The accompanying benchmark of 800 bug-fixing commits and 188 refactoring commits enables rigorous accuracy evaluation.[arxiv+1](https://arxiv.org/abs/2403.05939)​

## True Semantic Differencing (Behavioral Equivalence)

**SymDiff (2012) - "A Language-Agnostic Semantic Diff Tool for Imperative Programs"** by Lahiri et al. (Microsoft Research)[microsoft+1](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/paper-44.pdf)​

SymDiff performs actual **equivalence checking** using SMT solvers to determine if two program versions are behaviorally equivalent. Operating on Boogie intermediate language, it checks whether terminating executions under the same inputs produce identical outputs. When equivalence fails, it generates **abstract counterexample traces** showing the divergent paths.[acm+1](https://dl.acm.org/doi/10.1007/978-3-642-31424-7_54)​

**ARDiff (2020) - "Scaling Program Equivalence Checking via Iterative Abstraction and Refinement"** by Badihi et al.[ntu](https://www3.ntu.edu.sg/home/yi_li/files/Badihi2020ASP.pdf)​

This approach scales equivalence checking to larger programs through iterative abstraction. It addresses the computational challenge of semantic comparison by identifying common code portions that can be abstracted away.[ntu](https://www3.ntu.edu.sg/home/yi_li/files/Badihi2020ASP.pdf)​

**Semantic Program Alignment for Equivalence Checking (PLDI 2019)** by Churchill et al.[theory.stanford+1](https://theory.stanford.edu/~aiken/publications/papers/pldi19.pdf)​

This paper introduces a **trace alignment** technique driven by semantics rather than syntax. Given two functions and test cases, it builds a pairing of states across concrete executions and constructs a **product program** particularly amenable to proving equivalence. This approach extends equivalence checking to benchmarks beyond the reach of prior syntax-driven methods.[github+1](https://github.com/bchurchill/pldi19-equivalence-checker)​

**DIFFKEMP (2021) - "Automatically Checking Semantic Equivalence between Versions"** by Malík and Vojnar[fit.vut+1](https://www.fit.vut.cz/person/vojnar/public/Publications/mv-icst21-diffkemp.pdf)​

DIFFKEMP verifies whether two program versions that should have the same semantics (one being a refactoring of the other) actually do. It uses **semantics-preserving change patterns (SPCPs)** to recognize common transformations like constant propagation and dead code elimination.[ieeexplore.ieee+1](https://ieeexplore.ieee.org/document/9438578)​

## Higher-Level Differencing

**ClDiff (2018) - "Generating Concise Linked Code Differences"** by Huang et al.[chenbihuan.github+2](https://chenbihuan.github.io/paper/ase18-huang-cldiff.pdf)​

ClDiff operates at a granularity between fine-grained AST diff and code change summarization. It groups fine-grained changes at or above the **statement level** and establishes **links** between related changes using five pre-defined relationship types. This makes diffs more understandable for code review tasks.[acm+1](https://dl.acm.org/doi/10.1145/3238147.3238219)​

**Semantic Diff: A Tool for Summarizing the Effects of Modifications (1994)** by Jackson and Ladd[scribd+1](https://www.scribd.com/document/910022369/Semantic-Diff-a-tool-for-summarizing-the-effects-of-modifications)​

This classic paper from CMU/Bellcore describes a tool using **dependence relations** between inputs and outputs. Unlike syntactic tools, it can identify that two procedures produce equivalent computations even if they share almost no common syntax—as long as the dependences match.[ieeexplore.ieee+1](https://ieeexplore.ieee.org/document/336770/)​

## Tree Edit Distance Algorithms

The theoretical foundation for AST differencing comes from tree edit distance research:

- **RTED** - Robust algorithm with optimal O(n³) worst-case complexity[tree-edit-distance.dbresearch.uni-salzburg](http://tree-edit-distance.dbresearch.uni-salzburg.at/)​
    
- **Zhang-Shasha algorithm** - Classical O(n²m(1+log m/n)) approach[stackoverflow+1](https://stackoverflow.com/questions/1065247/how-do-i-calculate-tree-edit-distance)​
    
- **PQ-Gram approximation** - Fast approximation suitable for pre-filtering before exact computation[stackoverflow](https://stackoverflow.com/questions/1065247/how-do-i-calculate-tree-edit-distance)​
    

## Structural Diff Tools (Practical)

**Difftastic**[semanticdiff+3](https://semanticdiff.com/blog/semanticdiff-vs-difftastic/)​

A modern CLI tool using **tree-sitter** grammars to parse code and perform structural diffs. It understands when whitespace changes don't affect logic and can show actual semantic changes even when code is reformatted across lines. Supports 30+ languages.[sourceforge+2](https://sourceforge.net/projects/difftastic.mirror/)​

**SemanticDiff**[semanticdiff](https://semanticdiff.com/blog/semanticdiff-vs-difftastic/)​

A commercial tool that goes beyond structural diffs by attaching more semantic information to the AST and implementing rules to detect **invariant changes** (like adding optional parentheses) that don't change program logic.[semanticdiff](https://semanticdiff.com/blog/semanticdiff-vs-difftastic/)​

## Emerging Directions: Neural/Learning-Based

**SIGMADIFF** - "Semantics-Aware Deep Graph Matching for Pseudocode Diffing"[ndss-symposium](https://www.ndss-symposium.org/wp-content/uploads/2024-208-paper.pdf)​

Uses deep learning on decompiled pseudocode for binary diff, achieving token-level comparison with semantic understanding of the underlying code structure.[ndss-symposium](https://www.ndss-symposium.org/wp-content/uploads/2024-208-paper.pdf)​

**Code2Seq, Code2Vec, and GraphCode2Vec** approaches[escholarship+2](https://escholarship.org/content/qt8418w1c9/qt8418w1c9.pdf)​

These embed code into vector spaces using AST paths or program dependence graphs, enabling semantic similarity comparison. While not traditional diff tools, they represent code semantics in ways that could enhance diff algorithms.[arxiv+1](https://arxiv.org/pdf/2112.01218.pdf)​

## Recommended Papers by Use Case

|Use Case|Recommended Papers|
|---|---|
|Understanding code review changes|GumTree[acm](https://dl.acm.org/doi/10.1145/2642937.2642982)​, ClDiff[chenbihuan.github](https://chenbihuan.github.io/paper/ase18-huang-cldiff.pdf)​, RefactoringMiner AST diff[arxiv](https://arxiv.org/abs/2403.05939)​|
|Verifying refactoring correctness|DIFFKEMP[fit.vut](https://www.fit.vut.cz/person/vojnar/public/Publications/mv-icst21-diffkemp.pdf)​, SymDiff[microsoft](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/paper-44.pdf)​, Churchill et al.[theory.stanford](https://theory.stanford.edu/~aiken/publications/papers/pldi19.pdf)​|
|Extracting change patterns|ChangeDistiller[ieeexplore.ieee](https://ieeexplore.ieee.org/document/4339230/)​, MTDIFF[acm](https://dl.acm.org/doi/10.1145/2970276.2970315)​|
|Binary/decompiled code|SIGMADIFF[ndss-symposium](https://www.ndss-symposium.org/wp-content/uploads/2024-208-paper.pdf)​, MSSA[pmc.ncbi.nlm.nih](https://pmc.ncbi.nlm.nih.gov/articles/PMC11784775/)​|
|Theoretical foundations|Horwitz 1990[acm](https://dl.acm.org/doi/abs/10.1145/93548.93574)​, Jackson & Ladd 1994[scribd](https://www.scribd.com/document/910022369/Semantic-Diff-a-tool-for-summarizing-the-effects-of-modifications)​|

The field has evolved from pure syntactic comparison toward increasingly semantic understanding—from AST structure matching (GumTree) to refactoring awareness (RefactoringMiner) to full behavioral equivalence checking (SymDiff, ARDiff). Current research increasingly incorporates neural embeddings and LLM-based understanding of code semantics.[arxiv+1](https://arxiv.org/html/2504.04372v2)​

1. [https://dl.acm.org/doi/abs/10.1145/93548.93574](https://dl.acm.org/doi/abs/10.1145/93548.93574)
2. [https://dl.acm.org/doi/10.1145/93542.93574](https://dl.acm.org/doi/10.1145/93542.93574)
3. [https://cs.gmu.edu/~white/CS640/10.1.1.50.4405.pdf](https://cs.gmu.edu/~white/CS640/10.1.1.50.4405.pdf)
4. [https://research.cs.wisc.edu/wpis/papers/icse92.pdf](https://research.cs.wisc.edu/wpis/papers/icse92.pdf)
5. [https://dl.acm.org/doi/10.1145/2642937.2642982](https://dl.acm.org/doi/10.1145/2642937.2642982)
6. [https://www.semanticscholar.org/paper/Fine-grained-and-accurate-source-code-differencing-Falleri-Morandat/099c9fa379eeb10222969ce0073968ea47417768](https://www.semanticscholar.org/paper/Fine-grained-and-accurate-source-code-differencing-Falleri-Morandat/099c9fa379eeb10222969ce0073968ea47417768)
7. [https://courses.cs.vt.edu/cs6704/spring17/slides_by_students/CS6704_gumtree_Kijin_AN_Feb15.pdf](https://courses.cs.vt.edu/cs6704/spring17/slides_by_students/CS6704_gumtree_Kijin_AN_Feb15.pdf)
8. [https://xin-xia.github.io/publication/icse212.pdf](https://xin-xia.github.io/publication/icse212.pdf)
9. [https://ieeexplore.ieee.org/document/4339230/](https://ieeexplore.ieee.org/document/4339230/)
10. [https://ieeexplore.ieee.org/iel8/32/10930340/10873013.pdf](https://ieeexplore.ieee.org/iel8/32/10930340/10873013.pdf)
11. [https://notes.billmill.org/images/Change_DistillingTree_Differencing_for_Fine-Graine.pdf](https://notes.billmill.org/images/Change_DistillingTree_Differencing_for_Fine-Graine.pdf)
12. [https://www.ifi.uzh.ch/en/seal/research/tools/changeDistiller.html](https://www.ifi.uzh.ch/en/seal/research/tools/changeDistiller.html)
13. [https://www.aau.at/wp-content/uploads/2019/11/Gall2009-evolizer.pdf](https://www.aau.at/wp-content/uploads/2019/11/Gall2009-evolizer.pdf)
14. [https://dl.acm.org/doi/10.1145/2970276.2970315](https://dl.acm.org/doi/10.1145/2970276.2970315)
15. [https://ieeexplore.ieee.org/document/7582801/](https://ieeexplore.ieee.org/document/7582801/)
16. [https://github.com/FAU-Inf2/treedifferencing](https://github.com/FAU-Inf2/treedifferencing)
17. [https://www.xifiggam.eu/wp-content/uploads/2018/08/GeneratingAccurateandCompactEditScriptsusingTreeDifferencing.pdf](https://www.xifiggam.eu/wp-content/uploads/2018/08/GeneratingAccurateandCompactEditScriptsusingTreeDifferencing.pdf)
18. [https://dl.acm.org/doi/10.1145/3696002](https://dl.acm.org/doi/10.1145/3696002)
19. [https://ink.library.smu.edu.sg/context/sis_research/article/7882/viewcontent/A_Differential_Testing_Approach_for_Evaluating_Abstract_Syntax_Tree_Mapping_Algorithms.pdf](https://ink.library.smu.edu.sg/context/sis_research/article/7882/viewcontent/A_Differential_Testing_Approach_for_Evaluating_Abstract_Syntax_Tree_Mapping_Algorithms.pdf)
20. [https://arxiv.org/html/2411.07718v1](https://arxiv.org/html/2411.07718v1)
21. [https://arxiv.org/abs/2403.05939](https://arxiv.org/abs/2403.05939)
22. [https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/paper-44.pdf](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/paper-44.pdf)
23. [https://dl.acm.org/doi/10.1007/978-3-642-31424-7_54](https://dl.acm.org/doi/10.1007/978-3-642-31424-7_54)
24. [https://www3.ntu.edu.sg/home/yi_li/files/Badihi2020ASP.pdf](https://www3.ntu.edu.sg/home/yi_li/files/Badihi2020ASP.pdf)
25. [https://theory.stanford.edu/~aiken/publications/papers/pldi19.pdf](https://theory.stanford.edu/~aiken/publications/papers/pldi19.pdf)
26. [https://dl.acm.org/doi/10.1145/3314221.3314596](https://dl.acm.org/doi/10.1145/3314221.3314596)
27. [https://github.com/bchurchill/pldi19-equivalence-checker](https://github.com/bchurchill/pldi19-equivalence-checker)
28. [https://www.fit.vut.cz/person/vojnar/public/Publications/mv-icst21-diffkemp.pdf](https://www.fit.vut.cz/person/vojnar/public/Publications/mv-icst21-diffkemp.pdf)
29. [https://ieeexplore.ieee.org/document/9438578](https://ieeexplore.ieee.org/document/9438578)
30. [https://chenbihuan.github.io/paper/ase18-huang-cldiff.pdf](https://chenbihuan.github.io/paper/ase18-huang-cldiff.pdf)
31. [https://ieeexplore.ieee.org/document/9000085/](https://ieeexplore.ieee.org/document/9000085/)
32. [https://dl.acm.org/doi/10.1145/3238147.3238219](https://dl.acm.org/doi/10.1145/3238147.3238219)
33. [https://www.scribd.com/document/910022369/Semantic-Diff-a-tool-for-summarizing-the-effects-of-modifications](https://www.scribd.com/document/910022369/Semantic-Diff-a-tool-for-summarizing-the-effects-of-modifications)
34. [https://ieeexplore.ieee.org/document/336770/](https://ieeexplore.ieee.org/document/336770/)
35. [http://tree-edit-distance.dbresearch.uni-salzburg.at](http://tree-edit-distance.dbresearch.uni-salzburg.at/)
36. [https://stackoverflow.com/questions/1065247/how-do-i-calculate-tree-edit-distance](https://stackoverflow.com/questions/1065247/how-do-i-calculate-tree-edit-distance)
37. [https://semanticdiff.com/blog/semanticdiff-vs-difftastic/](https://semanticdiff.com/blog/semanticdiff-vs-difftastic/)
38. [https://sourceforge.net/projects/difftastic.mirror/](https://sourceforge.net/projects/difftastic.mirror/)
39. [https://difftastic.wilfred.me.uk](https://difftastic.wilfred.me.uk/)
40. [https://github.com/Wilfred/difftastic](https://github.com/Wilfred/difftastic)
41. [https://www.ndss-symposium.org/wp-content/uploads/2024-208-paper.pdf](https://www.ndss-symposium.org/wp-content/uploads/2024-208-paper.pdf)
42. [https://escholarship.org/content/qt8418w1c9/qt8418w1c9.pdf](https://escholarship.org/content/qt8418w1c9/qt8418w1c9.pdf)
43. [https://arxiv.org/pdf/2112.01218.pdf](https://arxiv.org/pdf/2112.01218.pdf)
44. [https://github.com/tech-srl/code2seq](https://github.com/tech-srl/code2seq)
45. [https://pmc.ncbi.nlm.nih.gov/articles/PMC11784775/](https://pmc.ncbi.nlm.nih.gov/articles/PMC11784775/)
46. [https://arxiv.org/html/2504.04372v2](https://arxiv.org/html/2504.04372v2)
47. [https://openreview.net/forum?id=82dbfUN3uf](https://openreview.net/forum?id=82dbfUN3uf)
48. [https://www.diva-portal.org/smash/get/diva2:210981/FULLTEXT01.pdf](https://www.diva-portal.org/smash/get/diva2:210981/FULLTEXT01.pdf)
49. [https://arxiv.org/abs/2503.20734](https://arxiv.org/abs/2503.20734)
50. [https://www.cs.ubc.ca/~rtholmes/papers/icsme_2019_hanam.pdf](https://www.cs.ubc.ca/~rtholmes/papers/icsme_2019_hanam.pdf)
51. [https://captain-whu.github.io/SCD/](https://captain-whu.github.io/SCD/)
52. [https://www.sciencedirect.com/science/article/abs/pii/S0164121223002510](https://www.sciencedirect.com/science/article/abs/pii/S0164121223002510)
53. [https://www.cs.umd.edu/~mwh/papers/evolution.pdf](https://www.cs.umd.edu/~mwh/papers/evolution.pdf)
54. [https://openaccess.thecvf.com/content/CVPR2025/papers/Benidir_The_Change_You_Want_To_Detect_Semantic_Change_Detection_In_CVPR_2025_paper.pdf](https://openaccess.thecvf.com/content/CVPR2025/papers/Benidir_The_Change_You_Want_To_Detect_Semantic_Change_Detection_In_CVPR_2025_paper.pdf)
55. [https://ieeexplore.ieee.org/document/10747244/](https://ieeexplore.ieee.org/document/10747244/)
56. [https://github.com/codinuum/diffast](https://github.com/codinuum/diffast)
57. [https://www.sciencedirect.com/science/article/abs/pii/S0924271624001709](https://www.sciencedirect.com/science/article/abs/pii/S0924271624001709)
58. [https://arxiv.org/abs/2409.13590](https://arxiv.org/abs/2409.13590)
59. [https://github.com/wenhwu/awesome-remote-sensing-change-detection](https://github.com/wenhwu/awesome-remote-sensing-change-detection)
60. [https://www.cs.utexas.edu/~isil/Revamp.pdf](https://www.cs.utexas.edu/~isil/Revamp.pdf)
61. [https://news.ycombinator.com/item?id=44520438](https://news.ycombinator.com/item?id=44520438)
62. [https://arxiv.org/abs/2011.10268](https://arxiv.org/abs/2011.10268)
63. [https://conf.researchr.org/details/icse-2024/icse-2024-research-track/148/Fine-grained-accurate-and-scalable-source-differencing](https://conf.researchr.org/details/icse-2024/icse-2024-research-track/148/Fine-grained-accurate-and-scalable-source-differencing)
64. [http://www.diva-portal.org/smash/get/diva2:1932698/FULLTEXT01.pdf](http://www.diva-portal.org/smash/get/diva2:1932698/FULLTEXT01.pdf)
65. [https://arxiv.org/html/2510.21094v1](https://arxiv.org/html/2510.21094v1)
66. [https://github.com/GumTreeDiff/gumtree](https://github.com/GumTreeDiff/gumtree)
67. [https://ieeexplore.ieee.org/document/10873013/](https://ieeexplore.ieee.org/document/10873013/)
68. [https://arxiv.org/html/2502.12466v1](https://arxiv.org/html/2502.12466v1)
69. [https://pure.tudelft.nl/admin/files/247344311/Scalable_Structural_Code_Diffs_Gumtree_Hybrid_v2-8-1.pdf](https://pure.tudelft.nl/admin/files/247344311/Scalable_Structural_Code_Diffs_Gumtree_Hybrid_v2-8-1.pdf)
70. [https://dl.acm.org/doi/fullHtml/10.1145/3564721.3564731](https://dl.acm.org/doi/fullHtml/10.1145/3564721.3564731)
71. [https://www.ijarcce.com/upload/2016/may-16/IJARCCE%2019.pdf](https://www.ijarcce.com/upload/2016/may-16/IJARCCE%2019.pdf)
72. [https://ceur-ws.org/Vol-1354/paper-04.pdf](https://ceur-ws.org/Vol-1354/paper-04.pdf)
73. [https://github.com/brunocodutra/tree-edit-distance](https://github.com/brunocodutra/tree-edit-distance)
74. [https://zhang-sai.github.io/pdf/li-stvr12.pdf](https://zhang-sai.github.io/pdf/li-stvr12.pdf)
75. [https://ldra.com/capabilities/change-impact-analysis-cia-capability/](https://ldra.com/capabilities/change-impact-analysis-cia-capability/)
76. [https://thume.ca/2017/06/17/tree-diffing/](https://thume.ca/2017/06/17/tree-diffing/)
77. [https://www.sciencedirect.com/science/article/abs/pii/S016412122030282X](https://www.sciencedirect.com/science/article/abs/pii/S016412122030282X)
78. [https://www.sciencedirect.com/science/article/pii/S2352220824000701](https://www.sciencedirect.com/science/article/pii/S2352220824000701)
79. [https://www.geeksforgeeks.org/dsa/edit-distance-dp-5/](https://www.geeksforgeeks.org/dsa/edit-distance-dp-5/)
80. [http://ieeexplore.ieee.org/document/8918947/](http://ieeexplore.ieee.org/document/8918947/)
81. [https://cgi.csc.liv.ac.uk/~coopes/comp319/2016/papers/ProgramSlicing-Binkley+Gallagher.pdf](https://cgi.csc.liv.ac.uk/~coopes/comp319/2016/papers/ProgramSlicing-Binkley+Gallagher.pdf)
82. [https://www.reddit.com/r/algorithms/comments/1oqw892/tree_edit_distance_where_connector_nodes_count_as/](https://www.reddit.com/r/algorithms/comments/1oqw892/tree_edit_distance_where_connector_nodes_count_as/)
83. [https://dl.acm.org/doi/10.1145/3643770](https://dl.acm.org/doi/10.1145/3643770)
84. [https://news.ycombinator.com/item?id=15101373](https://news.ycombinator.com/item?id=15101373)
85. [https://www.worldscientific.com/doi/10.1142/S0218194013500460](https://www.worldscientific.com/doi/10.1142/S0218194013500460)
86. [https://github.com/aatakansalar/llm-prompt-semantic-diff](https://github.com/aatakansalar/llm-prompt-semantic-diff)
87. [https://arxiv.org/pdf/2011.10268.pdf](https://arxiv.org/pdf/2011.10268.pdf)
88. [https://ieeexplore.ieee.org/servlet/Login?logout=%2Fdocument%2F9000085%2F](https://ieeexplore.ieee.org/servlet/Login?logout=%2Fdocument%2F9000085%2F)
89. [https://www.sciencedirect.com/science/article/abs/pii/S0950584925001193](https://www.sciencedirect.com/science/article/abs/pii/S0950584925001193)
90. [https://dl.acm.org/doi/abs/10.1145/3238147.3238219](https://dl.acm.org/doi/abs/10.1145/3238147.3238219)
91. [https://www.morphllm.com/edit-formats](https://www.morphllm.com/edit-formats)
92. [https://tabulareditor.com/blog/llms-and-semantic-models-complementary-technologies-for-enhanced-business-intelligence](https://tabulareditor.com/blog/llms-and-semantic-models-complementary-technologies-for-enhanced-business-intelligence)
93. [https://www.sciencedirect.com/science/article/abs/pii/S0164121223003072](https://www.sciencedirect.com/science/article/abs/pii/S0164121223003072)
94. [https://users.encs.concordia.ca/~nikolaos/theses/Pouria_Alikhanifard.pdf](https://users.encs.concordia.ca/~nikolaos/theses/Pouria_Alikhanifard.pdf)
95. [https://www.comsis.org/pdf.php?id=910-2306](https://www.comsis.org/pdf.php?id=910-2306)
96. [https://www.ijcai.org/Proceedings/99-2/Papers/062.pdf](https://www.ijcai.org/Proceedings/99-2/Papers/062.pdf)
97. [https://www.atlantis-press.com/article/25867868.pdf](https://www.atlantis-press.com/article/25867868.pdf)
98. [https://arxiv.org/pdf/1309.3730.pdf](https://arxiv.org/pdf/1309.3730.pdf)
99. [https://arxiv.org/abs/2402.00986](https://arxiv.org/abs/2402.00986)
100. [https://www.irif.fr/~thib/thesis.pdf](https://www.irif.fr/~thib/thesis.pdf)
101. [https://dl.acm.org/doi/10.1145/24039.24041](https://dl.acm.org/doi/10.1145/24039.24041)
102. [https://www2.ccs.neu.edu/racket/pubs/popl88-s.pdf](https://www2.ccs.neu.edu/racket/pubs/popl88-s.pdf)
103. [https://scholar.google.com/citations?user=btUSYPQAAAAJ&hl=en](https://scholar.google.com/citations?user=btUSYPQAAAAJ&hl=en)
104. [https://www.semanticscholar.org/paper/Semantical-Equivalence-of-the-Control-Flow-Graph-Ito/56e4e65065e87e42afaa0612e92d9bcd2ec120fc](https://www.semanticscholar.org/paper/Semantical-Equivalence-of-the-Control-Flow-Graph-Ito/56e4e65065e87e42afaa0612e92d9bcd2ec120fc)
105. [https://www.sciencedirect.com/science/article/pii/0167642395000186](https://www.sciencedirect.com/science/article/pii/0167642395000186)
106. [https://ieeexplore.ieee.org/document/8530035/](https://ieeexplore.ieee.org/document/8530035/)
107. [https://www.sei.cmu.edu/annual-reviews/2022-research-review/semantic-equivalence-checking-of-decompiled-binaries/](https://www.sei.cmu.edu/annual-reviews/2022-research-review/semantic-equivalence-checking-of-decompiled-binaries/)
108. [https://arxiv.org/pdf/2508.00749.pdf](https://arxiv.org/pdf/2508.00749.pdf)
109. [http://nickbenton.name/semdiff.pdf](http://nickbenton.name/semdiff.pdf)
110. [https://security.csl.toronto.edu/wp-content/uploads/2022/08/wwang_mascthesis_2022.pdf](https://security.csl.toronto.edu/wp-content/uploads/2022/08/wwang_mascthesis_2022.pdf)
111. [https://ksiresearch.org/seke/seke23paper/paper070.pdf](https://ksiresearch.org/seke/seke23paper/paper070.pdf)
112. [https://arxiv.org/html/2506.02290](https://arxiv.org/html/2506.02290)
113. [https://www.reddit.com/r/rust/comments/pp6y3d/difftastic_a_syntactic_diff_tool/](https://www.reddit.com/r/rust/comments/pp6y3d/difftastic_a_syntactic_diff_tool/)
114. [https://dl.acm.org/doi/10.1145/1453101.1453131](https://dl.acm.org/doi/10.1145/1453101.1453131)
115. [https://www.semanticscholar.org/paper/Differential-symbolic-execution-Person-Dwyer/38a0e40c5b04ebe9154390ccde800dc6cebbd6cb](https://www.semanticscholar.org/paper/Differential-symbolic-execution-Person-Dwyer/38a0e40c5b04ebe9154390ccde800dc6cebbd6cb)
116. [https://news.ycombinator.com/item?id=39778412](https://news.ycombinator.com/item?id=39778412)
117. [https://www.usenix.org/system/files/conference/usenixsecurity15/sec15-paper-ramos.pdf](https://www.usenix.org/system/files/conference/usenixsecurity15/sec15-paper-ramos.pdf)
118. [https://www.codemetal.ai/research/code-migration-with-formal-verification-for-performance-improvement-of-legacy-code](https://www.codemetal.ai/research/code-migration-with-formal-verification-for-performance-improvement-of-legacy-code)
119. [https://www.nature.com/articles/s41598-023-31280-w](https://www.nature.com/articles/s41598-023-31280-w)
120. [https://soheil-changizi.github.io/assets/docs/code2seq.pdf](https://soheil-changizi.github.io/assets/docs/code2seq.pdf)
121. [https://www.sciencedirect.com/science/article/pii/S258900422500416X](https://www.sciencedirect.com/science/article/pii/S258900422500416X)
122. [https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0305299](https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0305299)
123. [https://liner.com/review/code2seq-generating-sequences-from-structured-representations-of-code](https://liner.com/review/code2seq-generating-sequences-from-structured-representations-of-code)
124. [https://arxiv.org/html/2407.18877v1](https://arxiv.org/html/2407.18877v1)
125. [https://fluidattacks.com/blog/code-translate](https://fluidattacks.com/blog/code-translate)
126. [https://dl.acm.org/doi/abs/10.1145/3569933](https://dl.acm.org/doi/abs/10.1145/3569933)
127. A rich line of work in program analysis, refactoring, and ML-on-code looks exactly at “semantic” differencing rather than textual or purely AST-based diffs.[csaws.technion+1](https://csaws.cs.technion.ac.il/~yahave/papers/sas13-diff.pdf)​


----
# Research 2 

Below is a hand-picked list of papers and tools that are especially relevant if you want _high-quality semantic code diffs_, either via formal methods or learned representations.

## Classical semantic differencing

- **Horwitz, “Identifying the semantic and textual differences between programs” (POPL 1990)**  
    Foundational work that defines semantic differencing for programs and presents a technique to classify changes (e.g., changed procedures, unaffected parts) based on program dependence graphs.[acm](https://dl.acm.org/doi/abs/10.1145/93542.93574)​
    
- **Yahav et al., “Abstract Semantic Differencing for Numerical Programs” (SAS 2013)**  
    Uses abstract interpretation over a “correlating program” P▷◁P′P ▷◁ P'P▷◁P′ to compute sound over-approximations of behavioral differences between a program and its patched version, focused on numerical programs.[csaws.technion](https://csaws.cs.technion.ac.il/~yahave/papers/sas13-diff.pdf)​
    
- **Mohammed, “Semantic Differences and Graphical View of Programs” (M.Sc. thesis, 2009)**  
    Explores ways to locate the statements responsible for behavioral differences and visualize semantic differences, which is useful for understanding “what really changed” in terms of behavior.[diva-portal](https://www.diva-portal.org/smash/get/diva2:210981/FULLTEXT01.pdf)​
    

## Semantic AST / refactoring-aware diff

- **Alikhanifard & Tsantalis, “A Novel Refactoring and Semantic Aware Abstract Syntax Tree Differencer” (arXiv 2024 / TOSEM 2025)**  
    Proposes an AST diff tool built on RefactoringMiner that:
    
    - Handles multi-mapping
        
    - Avoids matching semantically incompatible nodes
        
    - Is explicitly refactoring-aware
        
    - Works at commit/pull-request level  
        They build a benchmark of AST node mappings on hundreds of bug-fix and refactoring commits and show better precision/recall than prior AST diff tools.[arxiv+1](https://arxiv.org/abs/2403.05939)​
        
- **Smart Differencer (Semantic Designs)**  
    Industrial tool that computes differences as abstract edit operations on a language-aware AST, marketing itself as “semantic differencing” for many languages; worth skimming its docs for design ideas even if you want an academic treatment.[semanticdesigns](https://semanticdesigns.com/Products/SmartDifferencer/)​
    

## Change embeddings and “semantic diff as representation”

- **Zhou et al., “Just-in-time defect prediction based on AST change embedding (ACE)” (Knowledge-Based Systems 2022)**  
    Introduces AST Change Embedding: a representation of code _changes_ (not snapshots) learned by comparing ASTs before and after a commit, explicitly to encode semantic aspects of the diff.[sciencedirect](https://www.sciencedirect.com/science/article/abs/pii/S0950705122004075)​
    
- **Tufano et al., “On Learning Meaningful Code Changes via Neural Machine Translation” (ICSE 2019)**  
    Trains NMT models to learn human code edits from version histories; qualitatively analyzes whether the learned edits reflect meaningful semantic changes rather than syntactic noise—useful if you want a learned model of “semantic diffs.”[semanticscholar](https://www.semanticscholar.org/paper/On-Learning-Meaningful-Code-Changes-Via-Neural-Tufano-Pantiuchina/b3a6e5a38bb5984a27823ecc040526cde10eb730)​
    
- **Ullah et al., “Learning Distributed Representation of Assembly for Binary Diffing” (2021)**  
    Focuses on _binary_ diffing, learning embeddings of assembly to capture functional similarity/differences between binaries; good inspiration if you care about semantics below source level.[ieeexplore.ieee](https://ieeexplore.ieee.org/document/9470904/)​
    

## Semantic graphs / clones closely related to semantic diff

- **Paiva et al., “Comparing Semantic Graph Representations of Source Code” (2024)**  
    Surveys and evaluates semantic graph representations (CFGs, PDGs, etc.) for reasoning about code semantics, which are often the substrate for semantic differencing and clone detection.[comsis](https://www.comsis.org/pdf.php?id=910-2306)​
    
- **Wu et al., “Detecting Semantic Code Clones by Building AST-based Markov Chains” (ASE 2022)**  
    Detects Type-4 (semantically similar, syntactically different) clones using an AST-based Markov model; the representation is effectively a semantic embedding of code that could be repurposed for semantic differencing.[wu-yueming.github](https://wu-yueming.github.io/Files/ASE2022_Amain.pdf)​
    
- **World Scientific 2024, “Semantic Code Clone Detection Based on Community Detection”**  
    Uses community detection over ASTs to extract semantic features of code blocks while preserving semantics; again, very close in spirit to semantic change/diff representations.[worldscientific](https://www.worldscientific.com/doi/10.1142/S0218194024500323)​
    

## For building a practical “semantic diff” tool

If you are thinking about implementing something:

- Use **refactoring-aware AST differencing** (Alikhanifard & Tsantalis; Smart Differencer) to get high-precision structural edits rather than line diffs.[arxiv+1](https://arxiv.org/abs/2403.05939)​
    
- Layer **abstract interpretation or symbolic execution** if you need formal behavioral difference guarantees (Yahav et al., Horwitz).[acm+1](https://dl.acm.org/doi/abs/10.1145/93542.93574)​
    
- Add **learned representations of edits** (ACE, NMT-based change learning) to capture higher-level semantics and rank/group diffs by behavioral impact.[semanticscholar+1](https://www.semanticscholar.org/paper/On-Learning-Meaningful-Code-Changes-Via-Neural-Tufano-Pantiuchina/b3a6e5a38bb5984a27823ecc040526cde10eb730)​
    

If you want, a follow-up can sketch a concrete architecture (e.g., for a Rust or Go tool) combining these ideas.

1. [https://csaws.cs.technion.ac.il/~yahave/papers/sas13-diff.pdf](https://csaws.cs.technion.ac.il/~yahave/papers/sas13-diff.pdf)
2. [https://arxiv.org/abs/2403.05939](https://arxiv.org/abs/2403.05939)
3. [https://dl.acm.org/doi/abs/10.1145/93542.93574](https://dl.acm.org/doi/abs/10.1145/93542.93574)
4. [https://www.diva-portal.org/smash/get/diva2:210981/FULLTEXT01.pdf](https://www.diva-portal.org/smash/get/diva2:210981/FULLTEXT01.pdf)
5. [https://dl.acm.org/doi/10.1145/3696002](https://dl.acm.org/doi/10.1145/3696002)
6. [https://semanticdesigns.com/Products/SmartDifferencer/](https://semanticdesigns.com/Products/SmartDifferencer/)
7. [https://www.sciencedirect.com/science/article/abs/pii/S0950705122004075](https://www.sciencedirect.com/science/article/abs/pii/S0950705122004075)
8. [https://www.semanticscholar.org/paper/On-Learning-Meaningful-Code-Changes-Via-Neural-Tufano-Pantiuchina/b3a6e5a38bb5984a27823ecc040526cde10eb730](https://www.semanticscholar.org/paper/On-Learning-Meaningful-Code-Changes-Via-Neural-Tufano-Pantiuchina/b3a6e5a38bb5984a27823ecc040526cde10eb730)
9. [https://ieeexplore.ieee.org/document/9470904/](https://ieeexplore.ieee.org/document/9470904/)
10. [https://www.comsis.org/pdf.php?id=910-2306](https://www.comsis.org/pdf.php?id=910-2306)
11. [https://wu-yueming.github.io/Files/ASE2022_Amain.pdf](https://wu-yueming.github.io/Files/ASE2022_Amain.pdf)
12. [https://www.worldscientific.com/doi/10.1142/S0218194024500323](https://www.worldscientific.com/doi/10.1142/S0218194024500323)
13. [http://ieeexplore.ieee.org/document/8115666/](http://ieeexplore.ieee.org/document/8115666/)
14. [https://www.geeksforgeeks.org/compiler-design/semantic-analysis-in-compiler-design/](https://www.geeksforgeeks.org/compiler-design/semantic-analysis-in-compiler-design/)
15. [https://zencoder.ai/blog/semantic-analysis-ai-code-generation](https://zencoder.ai/blog/semantic-analysis-ai-code-generation)
16. [https://arxiv.org/pdf/2505.18637.pdf](https://arxiv.org/pdf/2505.18637.pdf)
17. [https://ksiresearch.org/seke/seke22paper/paper039.pdf](https://ksiresearch.org/seke/seke22paper/paper039.pdf)
18. [https://ijcrt.org/papers/IJCRT2106719.pdf](https://ijcrt.org/papers/IJCRT2106719.pdf)
19. [https://www.sciencedirect.com/science/article/abs/pii/S0950584925001612](https://www.sciencedirect.com/science/article/abs/pii/S0950584925001612)
20. [https://dl.acm.org/doi/10.1145/3485135](https://dl.acm.org/doi/10.1145/3485135)