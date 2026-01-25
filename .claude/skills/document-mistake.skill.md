---
name: document-mistake
description: Document a coding mistake with structured analysis | Use after identifying an error
---

# Document Mistake Skill

## Workflow

### 1. Gather Info
- [ ] Problem ID or topic name
- [ ] What went wrong (1-2 line summary)
- [ ] Type: implementation | logic | conceptual | edge-case

### 2. Analyze
- [ ] Extract the buggy code snippet
- [ ] Identify root cause (not just symptom)
- [ ] Write corrected code
- [ ] Extract the key principle/lesson

### 3. Structure Document
- [ ] Frontmatter: tags array
- [ ] Header: # Problem [ID]: [Name]
- [ ] Section: Mistake (one-liner)
- [ ] Section: What Went Wrong (code + explanation)
- [ ] Section: Why (root cause, ~2-3 lines)
- [ ] Section: Fix (corrected code)
- [ ] Section: Key Lesson (takeaway principle)

### 4. Format Rules
- Use markdown code blocks with language tag
- Mark wrong code with ❌, right with ✅
- Tables for comparisons
- Bullets > prose
- Target length: ~150 words
- File: `[ID]_[name].md` or `[topic].md`

### 5. Checklist Before Save
- [ ] Frontmatter includes relevant tags
- [ ] Code examples are complete & runnable
- [ ] Root cause explained (not just "forgot X")
- [ ] Key lesson is generalizable (not problem-specific)
- [ ] No fluff, no "overview" sections

## Example Flow

**Input:** "I forgot to return from recursive base case"

**Output:**
```markdown
---
tags:
  - recursion
  - logic
---

# Recursion: Missing Base Case Return

## Mistake
Function implicitly returns None instead of early exit.

## What Went Wrong
❌ No return statement in base case

## Why
When recursion doesn't explicitly return, Python returns None. This breaks logic chains.

## Fix
✅ Add return statement in base case

## Key Lesson
Base cases must return appropriate value OR structure must not depend on return value.
```

## Tags Reference
```yaml
Common: leetcode, tree, recursion, dp, greedy, graph, string, array, logic, implementation, conceptual, edge-case, performance
Domain: data-structure, algorithm
```

---
*Skill: Document Mistake | Structured learning | Reusable patterns*
